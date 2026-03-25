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

**VB.NET:**
```vbnet
' Access the embedded Button control
Dim btn As Button = Me.editableList1.Button

' Configure button appearance
btn.Text = "Clear"
btn.Width = 70
btn.Height = 25
btn.BackColor = System.Drawing.Color.LightCoral
btn.ForeColor = System.Drawing.Color.White
btn.Font = New System.Drawing.Font("Arial", 9.0F, System.Drawing.FontStyle.Bold)
```

### Handling Button Events

```csharp
private void SetupButtonHandlers()
{
    // Handle button click
    this.editableList1.Button.Click += Button_Click;
    
    // Handle mouse events
    this.editableList1.Button.MouseEnter += (s, e) => {
        this.editableList1.Button.BackColor = System.Drawing.Color.Coral;
    };
    
    this.editableList1.Button.MouseLeave += (s, e) => {
        this.editableList1.Button.BackColor = System.Drawing.Color.LightCoral;
    };
}

private void Button_Click(object sender, EventArgs e)
{
    // Clear the editing TextBox
    this.editableList1.TextBox.Clear();
    
    // Or perform custom action
    MessageBox.Show("Button clicked during editing!");
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

// Set appearance
txt.Font = new System.Drawing.Font("Consolas", 10F);
txt.BackColor = System.Drawing.Color.LightYellow;
txt.ForeColor = System.Drawing.Color.Black;
txt.BorderStyle = BorderStyle.FixedSingle;

// Set behavior
txt.MaxLength = 100;
txt.CharacterCasing = CharacterCasing.Normal;
txt.TextAlign = HorizontalAlignment.Left;
```

**VB.NET:**
```vbnet
' Access the embedded TextBox
Dim txt As TextBox = Me.editableList1.TextBox

' Set appearance
txt.Font = New System.Drawing.Font("Consolas", 10.0F)
txt.BackColor = System.Drawing.Color.LightYellow
txt.ForeColor = System.Drawing.Color.Black
txt.BorderStyle = BorderStyle.FixedSingle

' Set behavior
txt.MaxLength = 100
txt.CharacterCasing = CharacterCasing.Normal
txt.TextAlign = HorizontalAlignment.Left
```

### Handling TextBox Events

```csharp
private void SetupTextBoxHandlers()
{
    // When editing starts
    this.editableList1.TextBox.Enter += (s, e) => {
        Console.WriteLine("Editing started");
        // Optionally select all text for easy replacement
        this.editableList1.TextBox.SelectAll();
    };
    
    // While typing
    this.editableList1.TextBox.TextChanged += (s, e) => {
        string currentText = this.editableList1.TextBox.Text;
        // Real-time validation or character counting
        Console.WriteLine($"Characters: {currentText.Length}");
    };
    
    // When editing completes
    this.editableList1.TextBox.Leave += TextBox_Leave;
    
    // Handle Enter key
    this.editableList1.TextBox.KeyPress += (s, e) => {
        if (e.KeyChar == (char)Keys.Enter)
        {
            e.Handled = true; // Prevent beep
            // Move focus to commit edit
            this.editableList1.ListBox.Focus();
        }
    };
}

private void TextBox_Leave(object sender, EventArgs e)
{
    // Validate input
    string text = this.editableList1.TextBox.Text.Trim();
    
    if (string.IsNullOrEmpty(text))
    {
        MessageBox.Show("Item cannot be empty!", "Validation Error");
    }
    else if (text.Length > 50)
    {
        MessageBox.Show("Item too long! Maximum 50 characters.", "Validation Error");
    }
}
```

### TextBox Input Validation

```csharp
private void RestrictTextBoxInput()
{
    // Allow only alphanumeric characters
    this.editableList1.TextBox.KeyPress += (s, e) => {
        if (!char.IsLetterOrDigit(e.KeyChar) && 
            !char.IsControl(e.KeyChar) && 
            e.KeyChar != ' ')
        {
            e.Handled = true; // Block the character
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
    
    // Item double-clicked
    this.editableList1.ListBox.DoubleClick += (s, e) => {
        if (this.editableList1.ListBox.SelectedItem != null)
        {
            // Trigger custom action
            ProcessSelectedItem();
        }
    };
    
    // Mouse interactions
    this.editableList1.ListBox.MouseHover += (s, e) => {
        // Show tooltip or preview
    };
}

private void ProcessSelectedItem()
{
    string item = this.editableList1.ListBox.SelectedItem.ToString();
    MessageBox.Show($"Processing: {item}");
}
```

### Custom Drawing (Advanced)

```csharp
private void EnableCustomDrawing()
{
    // Enable owner-draw mode
    this.editableList1.ListBox.DrawMode = DrawMode.OwnerDrawFixed;
    this.editableList1.ListBox.DrawItem += ListBox_DrawItem;
}

private void ListBox_DrawItem(object sender, DrawItemEventArgs e)
{
    if (e.Index < 0) return;
    
    e.DrawBackground();
    
    // Custom drawing logic
    string itemText = this.editableList1.ListBox.Items[e.Index].ToString();
    Brush brush = (e.State & DrawItemState.Selected) == DrawItemState.Selected 
        ? Brushes.White 
        : Brushes.Black;
    
    e.Graphics.DrawString(itemText, e.Font, brush, e.Bounds);
    e.DrawFocusRectangle();
}
```

## AutoScroll Configuration

AutoScroll automatically shows scrollbars when the list content exceeds the control's visible area.

### Enabling AutoScroll

```csharp
// Enable automatic scrollbars
this.editableList1.AutoScroll = true;
```

**VB.NET:**
```vbnet
' Enable automatic scrollbars
Me.editableList1.AutoScroll = True
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
    // Enable AutoScroll
    this.editableList1.AutoScroll = true;
    
    // Set margin (space around controls during autoscroll)
    this.editableList1.AutoScrollMargin = new System.Drawing.Size(2, 2);
    
    // Set minimum size for autoscroll region
    this.editableList1.AutoScrollMinSize = new System.Drawing.Size(3, 3);
}
```

**VB.NET:**
```vbnet
Private Sub ConfigureAutoScroll()
    ' Enable AutoScroll
    Me.editableList1.AutoScroll = True
    
    ' Set margin (space around controls during autoscroll)
    Me.editableList1.AutoScrollMargin = New System.Drawing.Size(2, 2)
    
    ' Set minimum size for autoscroll region
    Me.editableList1.AutoScrollMinSize = New System.Drawing.Size(3, 3)
End Sub
```

### When to Use AutoScroll

**Recommended scenarios:**
- Lists with variable number of items (may grow beyond visible area)
- Fixed-size container with potentially large content
- Forms where EditableList size is constrained by layout

**Example - Large List:**
```csharp
private void PopulateLargeList()
{
    // Enable autoscroll before adding many items
    this.editableList1.AutoScroll = true;
    
    // Add 100 items
    for (int i = 1; i <= 100; i++)
    {
        this.editableList1.ListBox.Items.Add($"Item {i}");
    }
    
    // Scrollbars appear automatically if needed
}
```

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

**VB.NET:**
```vbnet
' Set padding for all edges equally
Me.editableList1.DockPadding.All = 5

' Or set each edge individually
Me.editableList1.DockPadding.Left = 10
Me.editableList1.DockPadding.Right = 10
Me.editableList1.DockPadding.Top = 5
Me.editableList1.DockPadding.Bottom = 5
```

### Visual Effect

```csharp
// Create visual breathing room
private void ApplySpacedLayout()
{
    // Add 10 pixels of space on all sides
    this.editableList1.DockPadding.All = 10;
    
    // This creates space between:
    // - Control border and ListBox
    // - Control edges and embedded controls
}
```

### Use Cases for DockPadding

1. **Visual Separation:** Create space around the list for better aesthetics
2. **Form Layout:** Match padding with other controls on the form
3. **Border Effects:** Combine with BackColor for border-like appearance

```csharp
// Example: Create border effect
this.editableList1.BackColor = System.Drawing.Color.DarkGray;
this.editableList1.DockPadding.All = 2; // 2-pixel "border"
this.editableList1.ListBox.BackColor = System.Drawing.Color.White;
```

## WantButton Property

The `WantButton` property controls whether the Button control is visible during editing operations.

### Enabling/Disabling Button

```csharp
// Show button while editing
this.editableList1.WantButton = true;

// Hide button (button not visible during editing)
this.editableList1.WantButton = false;
```

**VB.NET:**
```vbnet
' Show button while editing
Me.editableList1.WantButton = True

' Hide button (button not visible during editing)
Me.editableList1.WantButton = False
```

### When Button Appears

When `WantButton = true`:
1. User selects an item
2. User clicks again to edit
3. **TextBox AND Button both appear**
4. Button is positioned to the right of the TextBox

### Practical Use Cases

**Use Case 1: Clear Button**
```csharp
private void SetupClearButton()
{
    this.editableList1.WantButton = true;
    this.editableList1.Button.Text = "✕";
    this.editableList1.Button.Width = 30;
    
    this.editableList1.Button.Click += (s, e) => {
        this.editableList1.TextBox.Clear();
        this.editableList1.TextBox.Focus();
    };
}
```

**Use Case 2: Accept/Submit Button**
```csharp
private void SetupAcceptButton()
{
    this.editableList1.WantButton = true;
    this.editableList1.Button.Text = "✓";
    
    this.editableList1.Button.Click += (s, e) => {
        // Validate and commit
        if (ValidateInput(this.editableList1.TextBox.Text))
        {
            this.editableList1.ListBox.Focus(); // Commits edit
        }
    };
}

private bool ValidateInput(string text)
{
    if (string.IsNullOrWhiteSpace(text))
    {
        MessageBox.Show("Input cannot be empty!");
        return false;
    }
    return true;
}
```

**Use Case 3: Delete Button**
```csharp
private void SetupDeleteButton()
{
    this.editableList1.WantButton = true;
    this.editableList1.Button.Text = "Delete";
    this.editableList1.Button.BackColor = System.Drawing.Color.Red;
    this.editableList1.Button.ForeColor = System.Drawing.Color.White;
    
    this.editableList1.Button.Click += (s, e) => {
        int index = this.editableList1.ListBox.SelectedIndex;
        if (index >= 0)
        {
            this.editableList1.ListBox.Items.RemoveAt(index);
        }
    };
}
```

## Control Sizing and Layout

Proper sizing ensures EditableList displays correctly and provides good user experience.

### Basic Sizing

```csharp
// Set control size
this.editableList1.Size = new System.Drawing.Size(300, 250);
// Width: 300 pixels, Height: 250 pixels

// Set position
this.editableList1.Location = new System.Drawing.Point(20, 50);
// X: 20 pixels from left, Y: 50 pixels from top
```

### Sizing Recommendations

| Scenario | Recommended Size | Rationale |
|----------|------------------|-----------|
| Small list (5-10 items) | 250×150 | Compact, fits most forms |
| Medium list (10-20 items) | 300×250 | Balanced size, good visibility |
| Large list (20+ items) | 350×350+ | Needs more height, enable AutoScroll |
| Full form width | Width=FormWidth-40 | 20px margin each side |

### Responsive Sizing

```csharp
// Anchor to resize with form
this.editableList1.Anchor = AnchorStyles.Top | AnchorStyles.Bottom | 
                             AnchorStyles.Left | AnchorStyles.Right;

// Dock to fill area
this.editableList1.Dock = DockStyle.Fill;
```

### Minimum and Maximum Sizes

```csharp
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

### Example 2: Custom Styled EditableList

```csharp
private void CreateStyledList()
{
    var editableList = new EditableList();
    editableList.Location = new Point(30, 30);
    editableList.Size = new Size(320, 280);
    
    // Background colors
    editableList.BackColor = Color.FromArgb(240, 240, 245);
    editableList.ListBox.BackColor = Color.White;
    editableList.TextBox.BackColor = Color.LightYellow;
    
    // Fonts
    var listFont = new Font("Segoe UI", 10F, FontStyle.Regular);
    editableList.ListBox.Font = listFont;
    editableList.TextBox.Font = new Font("Segoe UI", 10F, FontStyle.Italic);
    
    // Border-like effect with DockPadding
    editableList.DockPadding.All = 3;
    
    // Custom button
    editableList.WantButton = true;
    editableList.Button.Text = "⊗";
    editableList.Button.BackColor = Color.LightCoral;
    editableList.Button.ForeColor = Color.White;
    editableList.Button.Font = new Font("Segoe UI", 12F, FontStyle.Bold);
    
    this.Controls.Add(editableList);
}
```

## Troubleshooting

**Issue:** Button not appearing during edit  
**Solution:** Set `WantButton = true` property

**Issue:** Scrollbars not showing with many items  
**Solution:** Enable `AutoScroll = true` and ensure control has fixed size

**Issue:** Embedded controls not responding to events  
**Solution:** Ensure event handlers are attached after control initialization

**Issue:** DockPadding has no visible effect  
**Solution:** Check that BackColor is different from embedded control colors to see the padding space

**Issue:** Layout looks cramped  
**Solution:** Adjust `DockPadding.All` and control `Size` properties
