# Child Control Constraints

## Table of Contents
- [Constraints Overview](#constraints-overview)
- [FlowLayoutConstraints Structure](#flowlayoutconstraints-structure)
- [HAlign and VAlign](#halign-and-valign)
- [Active Property](#active-property)
- [NewLine Property](#newline-property)
- [Proportional Sizing](#proportional-sizing)
- [Constraint Methods](#constraint-methods)
- [Complete Use Cases](#complete-use-cases)
- [Rearranging Controls](#rearranging-controls)

---

## Constraints Overview

Child control constraints allow fine-grained control over how individual controls are positioned and sized within the FlowLayout. This enables complex, responsive layouts beyond simple uniform alignment.

### When to Use Constraints

- Building complex data entry forms with labels and inputs
- Creating layouts where controls have different sizing requirements
- Implementing constraint-based (rather than simple) alignment
- Resizing specific controls to fill available space
- Creating responsive forms that adapt to container resizing

### Constraint-Based Layout Example

```csharp
// Instead of uniform alignment:
flowLayout1.Alignment = FlowAlignment.Center;  // All controls center

// Use per-control constraints:
flowLayout1.Alignment = FlowAlignment.ChildConstraints;
// Each control can have different alignment and sizing
```

## FlowLayoutConstraints Structure

The `FlowLayoutConstraints` type defines constraint properties for a single control:

```csharp
var constraints = new FlowLayoutConstraints(
    active: true,                          // Include in layout
    hAlign: HorzFlowAlign.Justify,         // Horizontal alignment
    vAlign: VertFlowAlign.Center,          // Vertical alignment
    newLine: false,                        // Force new row/column
    proportionalColWidth: false,           // Vertical mode: use proportional width
    proportionalRowHeight: false           // Horizontal mode: use proportional height
);

flowLayout1.SetConstraints(controlName, constraints);
```

### Constructor Parameters

| Parameter | Type | Purpose |
|-----------|------|---------|
| `active` | bool | Whether control participates in layout |
| `hAlign` | HorzFlowAlign | Horizontal alignment: Left, Right, Center, Justify |
| `vAlign` | VertFlowAlign | Vertical alignment: Top, Bottom, Center, Justify |
| `newLine` | bool | Force this control to start a new row/column |
| `proportionalColWidth` | bool | (Vertical mode) Use proportional column width |
| `proportionalRowHeight` | bool | (Horizontal mode) Use proportional row height |

## HAlign and VAlign

Horizontal and vertical alignment determine how a control is positioned within its row or column.

### HorzFlowAlign Options

```csharp
public enum HorzFlowAlign
{
    Left,      // Align to left
    Right,     // Align to right
    Center,    // Center horizontally
    Justify    // Stretch to fill available horizontal space
}
```

### VertFlowAlign Options

```csharp
public enum VertFlowAlign
{
    Top,       // Align to top
    Bottom,    // Align to bottom
    Center,    // Center vertically
    Justify    // Stretch to fill available vertical space
}
```

### Horizontal Layout Alignment

In horizontal mode (controls flow left-to-right):

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.Alignment = FlowAlignment.ChildConstraints;

// Label: Left-align in row, center vertically
var labelConstraints = new FlowLayoutConstraints(
    true, HorzFlowAlign.Left, VertFlowAlign.Center, false, false, false);
flowLayout1.SetConstraints(label, labelConstraints);

// TextBox: Stretch horizontally, center vertically
var textBoxConstraints = new FlowLayoutConstraints(
    true, HorzFlowAlign.Justify, VertFlowAlign.Center, false, false, false);
flowLayout1.SetConstraints(textBox, textBoxConstraints);
```

### Vertical Layout Alignment

In vertical mode (controls flow top-to-bottom):

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Vertical;
flowLayout1.Alignment = FlowAlignment.ChildConstraints;

// Narrow control: Left-align in column, top-align vertically
var narrowConstraints = new FlowLayoutConstraints(
    true, HorzFlowAlign.Left, VertFlowAlign.Top, false, false, false);
flowLayout1.SetConstraints(checkBox, narrowConstraints);

// Wide control: Stretch width in column, center vertically
var wideConstraints = new FlowLayoutConstraints(
    true, HorzFlowAlign.Justify, VertFlowAlign.Center, false, false, false);
flowLayout1.SetConstraints(textBox, wideConstraints);
```

## Active Property

The `Active` property determines whether a control participates in the layout:

```csharp
var constraints = new FlowLayoutConstraints(
    active: true,   // Participates in layout
    hAlign: HorzFlowAlign.Left,
    vAlign: VertFlowAlign.Top,
    newLine: false,
    proportionalColWidth: false,
    proportionalRowHeight: false
);
flowLayout1.SetConstraints(control, constraints);
```

### Setting Active = false

Excludes a control from layout without removing it from the form:

```csharp
var inactiveConstraints = new FlowLayoutConstraints(
    active: false,  // Don't include in layout
    hAlign: HorzFlowAlign.Left,
    vAlign: VertFlowAlign.Top,
    newLine: false,
    proportionalColWidth: false,
    proportionalRowHeight: false
);
flowLayout1.SetConstraints(control, inactiveConstraints);
```

### Use Cases

**Conditional Visibility:**
```csharp
// Show/hide control without removing it
bool showAdvanced = userEnabled;
var constraints = new FlowLayoutConstraints(
    active: showAdvanced,
    hAlign: HorzFlowAlign.Left,
    vAlign: VertFlowAlign.Top,
    newLine: false, false, false);
flowLayout1.SetConstraints(advancedPanel, constraints);
```

**Temporarily Disable Layout:**
```csharp
// Disable several controls during updates
foreach (Control ctrl in controlsToHide)
{
    var constraints = new FlowLayoutConstraints(
        false, HorzFlowAlign.Left, VertFlowAlign.Top, false, false, false);
    flowLayout1.SetConstraints(ctrl, constraints);
}
```

## NewLine Property

The `NewLine` property forces a control to start a new row (horizontal mode) or new column (vertical mode):

```csharp
var constraints = new FlowLayoutConstraints(
    active: true,
    hAlign: HorzFlowAlign.Left,
    vAlign: VertFlowAlign.Top,
    newLine: true,   // Force new row/column
    proportionalColWidth: false,
    proportionalRowHeight: false
);
flowLayout1.SetConstraints(breakControl, constraints);
```

### Horizontal Mode Example

Forces a control to start a new row:

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;

// Controls 1-3 in first row
Button btn1 = new Button { Text = "1", Size = new Size(60, 30) };
Button btn2 = new Button { Text = "2", Size = new Size(60, 30) };
Button btn3 = new Button { Text = "3", Size = new Size(60, 30) };

this.Controls.Add(btn1);
this.Controls.Add(btn2);
this.Controls.Add(btn3);

// Control 4 forced to new row
Button btn4 = new Button { Text = "4", Size = new Size(60, 30) };
this.Controls.Add(btn4);

// Set NewLine on button4
var constraints = new FlowLayoutConstraints(
    true, HorzFlowAlign.Left, VertFlowAlign.Top, true, false, false);
flowLayout1.SetConstraints(btn4, constraints);

// Result:
// [1] [2] [3]
// [4]
```

### Use Cases

**Section Breaks:**
```csharp
// Separate groups of controls
var sectionConstraints = new FlowLayoutConstraints(
    true, HorzFlowAlign.Left, VertFlowAlign.Top, true, false, false);
flowLayout1.SetConstraints(section2Panel, sectionConstraints);
```

**Form Field Rows:**
```csharp
// Each form row starts on new line
foreach (TextBox textBox in textBoxes)
{
    var constraints = new FlowLayoutConstraints(
        true, HorzFlowAlign.Justify, VertFlowAlign.Top, true, false, false);
    flowLayout1.SetConstraints(textBox, constraints);
}
```

## Proportional Sizing

Proportional sizing adjusts control height (horizontal mode) or width (vertical mode) to use available space.

### ProportionalRowHeight (Horizontal Mode)

In horizontal layout, controls in a row normally have fixed height. Enable `ProportionalRowHeight` to stretch:

```csharp
var constraints = new FlowLayoutConstraints(
    active: true,
    hAlign: HorzFlowAlign.Left,
    vAlign: VertFlowAlign.Center,
    newLine: false,
    proportionalColWidth: false,
    proportionalRowHeight: true   // Stretch vertically
);
flowLayout1.SetConstraints(control, constraints);
```

**Effect:** Control height increases proportionally to fill available vertical space in row.

### ProportionalColWidth (Vertical Mode)

In vertical layout, controls in a column normally have fixed width. Enable `ProportionalColWidth` to stretch:

```csharp
var constraints = new FlowLayoutConstraints(
    active: true,
    hAlign: HorzFlowAlign.Center,
    vAlign: VertFlowAlign.Top,
    newLine: false,
    proportionalColWidth: true,   // Stretch horizontally
    proportionalRowHeight: false
);
flowLayout1.SetConstraints(control, constraints);
```

**Effect:** Control width increases proportionally to fill available horizontal space in column.

### Example: Vertically Centered Row

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;

Label label = new Label { Text = "Name:", Size = new Size(50, 20) };
TextBox textBox = new TextBox { Size = new Size(150, 20) };

this.Controls.Add(label);
this.Controls.Add(textBox);

// Make textBox row-height proportional for vertical centering
var textBoxConstraints = new FlowLayoutConstraints(
    true, HorzFlowAlign.Left, VertFlowAlign.Center, false, false, true);
flowLayout1.SetConstraints(textBox, textBoxConstraints);
```

## Constraint Methods

### SetConstraints

Set all constraints for a control:

```csharp
flowLayout1.SetConstraints(control, constraints);
```

**C# Example:**
```csharp
var constraints = new FlowLayoutConstraints(
    true, HorzFlowAlign.Justify, VertFlowAlign.Center, 
    false, false, false);
flowLayout1.SetConstraints(textBox1, constraints);
```

**VB.NET Example:**
```vb
Dim constraints As New FlowLayoutConstraints(
    True, HorzFlowAlign.Justify, VertFlowAlign.Center, 
    False, False, False)
flowLayout1.SetConstraints(textBox1, constraints)
```

### GetConstraints

Retrieve constraints for a control:

```csharp
FlowLayoutConstraints constraints = flowLayout1.GetConstraints(control);

if (constraints.Active)
{
    // Control participates in layout
}
```

### GetConstraintsRef

Get a reference to constraints for modification:

```csharp
FlowLayoutConstraints constraintsRef = 
    flowLayout1.GetConstraintsRef(control);

// Modify reference
constraintsRef.HAlign = HorzFlowAlign.Justify;
```

### SetPreferredSize

Set the preferred size for a control (used during constraint-based layout):

```csharp
flowLayout1.SetPreferredSize(textBox1, new Size(200, 25));
```

## Complete Use Cases

### User Info Form

Build a responsive data entry form with labels and text boxes:

```csharp
using Syncfusion.Windows.Forms.Tools;

public partial class UserInfoForm : Form
{
    private FlowLayout flowLayout1;
    private Panel infoPanel;
    
    public UserInfoForm()
    {
        InitializeComponent();
        
        // Create panel and layout
        infoPanel = new Panel { Dock = DockStyle.Fill };
        this.Controls.Add(infoPanel);
        
        flowLayout1 = new FlowLayout();
        flowLayout1.ContainerControl = infoPanel;
        flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
        flowLayout1.Alignment = FlowAlignment.ChildConstraints;
        flowLayout1.AutoHeight = true;
        flowLayout1.HGap = 5;
        flowLayout1.VGap = 8;
        
        // Add fields
        AddField("First Name:", "textFirstName");
        AddField("Last Name:", "textLastName");
        AddField("Email:", "textEmail");
        AddField("Address:", "textAddress");
    }
    
    private void AddField(string labelText, string textBoxName)
    {
        // Label - fixed width, left-aligned
        Label label = new Label
        {
            Text = labelText + ":",
            Size = new Size(80, 25),
            TextAlign = ContentAlignment.MiddleLeft
        };
        
        // TextBox - stretches to fill width
        TextBox textBox = new TextBox
        {
            Name = textBoxName,
            Size = new Size(200, 25),
            Multiline = false
        };
        
        // Add to panel
        infoPanel.Controls.Add(label);
        infoPanel.Controls.Add(textBox);
        
        // Set constraints
        var labelConstraints = new FlowLayoutConstraints(
            true, HorzFlowAlign.Left, VertFlowAlign.Center, 
            false, false, false);
        flowLayout1.SetConstraints(label, labelConstraints);
        
        // Stretch textbox, force new line
        var textBoxConstraints = new FlowLayoutConstraints(
            true, HorzFlowAlign.Justify, VertFlowAlign.Center, 
            true, false, false);
        flowLayout1.SetConstraints(textBox, textBoxConstraints);
        
        // Set preferred size for textbox
        flowLayout1.SetPreferredSize(textBox, new Size(150, 25));
    }
}
```

**Result:**
```
[First Name:] [TextBox________________________]
[Last Name:] [TextBox________________________]
[Email:] [TextBox________________________]
[Address:] [TextBox________________________]
```

### Complex Layout with Multiple Rows

```csharp
// Row 1: Name fields
Label firstName = new Label { Text = "First Name:" };
TextBox firstNameBox = new TextBox { Size = new Size(80, 25) };
Label lastName = new Label { Text = "Last Name:" };
TextBox lastNameBox = new TextBox { Size = new Size(80, 25) };

// Row 2: Contact info
Label email = new Label { Text = "Email:" };
TextBox emailBox = new TextBox { Size = new Size(200, 25) };

// Row 3: Address (wide)
Label address = new Label { Text = "Address:" };
TextBox addressBox = new TextBox { Size = new Size(250, 25) };

// Add all controls
panel.Controls.AddRange(new Control[] { 
    firstName, firstNameBox, lastName, lastNameBox,
    email, emailBox, address, addressBox 
});

// Configure layout
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.Alignment = FlowAlignment.ChildConstraints;
flowLayout1.HGap = 10;
flowLayout1.VGap = 10;

// Row 1 - inline fields
flowLayout1.SetConstraints(firstNameBox, new FlowLayoutConstraints(
    true, HorzFlowAlign.Left, VertFlowAlign.Top, false, false, false));
flowLayout1.SetConstraints(lastNameBox, new FlowLayoutConstraints(
    true, HorzFlowAlign.Left, VertFlowAlign.Top, false, false, false));

// Row 2 - new row, stretched
flowLayout1.SetConstraints(emailBox, new FlowLayoutConstraints(
    true, HorzFlowAlign.Justify, VertFlowAlign.Top, true, false, false));

// Row 3 - new row, stretched
flowLayout1.SetConstraints(addressBox, new FlowLayoutConstraints(
    true, HorzFlowAlign.Justify, VertFlowAlign.Top, true, false, false));
```

## Rearranging Controls

### Rearranging via Designer

1. Right-click on a control
2. Select "Bring to Front" to move earlier in layout order
3. Select "Send to Back" to move later in layout order

### Rearranging Programmatically

Reorder controls by manipulating the Controls collection:

```csharp
private void ReverseControlOrder()
{
    // Collect current controls
    ArrayList controlList = new ArrayList();
    foreach (Control ctrl in panel1.Controls)
    {
        controlList.Add(ctrl);
    }
    
    // Clear and re-add in reverse order
    panel1.Controls.Clear();
    
    for (int i = controlList.Count - 1; i >= 0; i--)
    {
        panel1.Controls.Add((Control)controlList[i]);
    }
    
    // Refresh layout
    panel1.PerformLayout();
}
```

### Moving Specific Control

```csharp
private void MoveControlToFront(Control control)
{
    container.Controls.Remove(control);
    container.Controls.Insert(0, control);
    container.PerformLayout();
}

private void MoveControlToBack(Control control)
{
    container.Controls.Remove(control);
    container.Controls.Add(control);
    container.PerformLayout();
}
```

### Reordering at Runtime

```csharp
private void ReorderButton_Click(object sender, EventArgs e)
{
    // Swap positions of two controls
    int index1 = container.Controls.IndexOf(control1);
    int index2 = container.Controls.IndexOf(control2);
    
    if (index1 >= 0 && index2 >= 0)
    {
        container.Controls.RemoveAt(Math.Max(index1, index2));
        container.Controls.RemoveAt(Math.Min(index1, index2));
        
        container.Controls.Insert(0, control1);
        container.Controls.Insert(1, control2);
        
        container.PerformLayout();
    }
}
```

## Troubleshooting

### Controls Not Participating in Layout

**Problem:** Added controls don't appear in layout
**Solution:** Check if `Active` property is set to `true` in constraints

```csharp
// Verify control is active
var constraints = flowLayout1.GetConstraints(control);
if (!constraints.Active)
{
    // Make active
    var activeConstraints = new FlowLayoutConstraints(
        true, HorzFlowAlign.Left, VertFlowAlign.Top, false, false, false);
    flowLayout1.SetConstraints(control, activeConstraints);
}
```

### Unexpected Layout Changes

**Problem:** Layout rearranges unexpectedly when changing properties
**Solution:** Ensure `Alignment` is correct and constraints are consistent

```csharp
// Use ChildConstraints if mixing alignment styles
flowLayout1.Alignment = FlowAlignment.ChildConstraints;

// Verify all controls have defined constraints
foreach (Control ctrl in container.Controls)
{
    var constraints = flowLayout1.GetConstraints(ctrl);
    // Should be non-null if using ChildConstraints alignment
}
```
