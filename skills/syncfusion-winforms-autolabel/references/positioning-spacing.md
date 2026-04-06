# Positioning and Spacing in Windows Forms AutoLabel

This section explains how to control the position and spacing of AutoLabel controls relative to their labeled controls.

## Position Property

The AutoLabel control can be positioned relative to the top, left, bottom, or right of the labeled control using the `Position` property.

| Position Value | Description | Usage |
|---------------|-------------|-------|
| `AutoLabelPosition.Left` | Label appears to the left of the control | Standard horizontal forms |
| `AutoLabelPosition.Top` | Label appears above the control | Vertical/stacked forms |
| `AutoLabelPosition.Side` | Label appears on the side (right) | Alternative horizontal layout |
| `AutoLabelPosition.Custom` | Manual positioning with DX/DY | Special layouts requiring precise control |

### Example: Left Position

```csharp
this.autoLabel1.Position = Syncfusion.Windows.Forms.Tools.AutoLabelPosition.Left;
```

```vb
Me.autoLabel1.Position = Syncfusion.Windows.Forms.Tools.AutoLabelPosition.Left
```

The label will appear to the left of the labeled control with spacing controlled by the `Gap` property.

### Example: Top Position

```csharp
this.autoLabel1.Position = Syncfusion.Windows.Forms.Tools.AutoLabelPosition.Top;
```

```vb
Me.autoLabel1.Position = Syncfusion.Windows.Forms.Tools.AutoLabelPosition.Top
```

The label will appear above the labeled control, ideal for vertical form layouts.

### Example: Side Position

```csharp
this.autoLabel1.Position = Syncfusion.Windows.Forms.Tools.AutoLabelPosition.Side;
```

```vb
Me.autoLabel1.Position = Syncfusion.Windows.Forms.Tools.AutoLabelPosition.Side
```

The label will appear on the right side of the labeled control.

### Example: Custom Position

When the Position property is set to **Custom**, you can drag the label to the required position using the mouse or control it programmatically with DX and DY properties.

```csharp
this.autoLabel1.Position = Syncfusion.Windows.Forms.Tools.AutoLabelPosition.Custom;
this.autoLabel1.DX = -100;  // Position 100 pixels to the left
this.autoLabel1.DY = 10;    // Position 10 pixels down
```

```vb
Me.autoLabel1.Position = Syncfusion.Windows.Forms.Tools.AutoLabelPosition.Custom
Me.autoLabel1.DX = -100  ' Position 100 pixels to the left
Me.autoLabel1.DY = 10    ' Position 10 pixels down
```

## Spacing Properties

The space between the AutoLabel control and the labeled control can be customized using the following properties.

| Property | Type | Description |
|----------|------|-------------|
| `Gap` | int | Horizontal and vertical gap when using relative positioning (Left, Top, Side) |
| `DX` | int | The effective horizontal distance between the left of the AutoLabel and its labeled control |
| `DY` | int | The effective vertical distance between the top of the AutoLabel and its labeled control |

### Gap Property

When using relative positioning (Left, Top, or Side), you can specify the gap between the label and the control.

```csharp
this.autoLabel1.Gap = 10;  // 10 pixels gap between label and control
```

```vb
Me.autoLabel1.Gap = 10  ' 10 pixels gap between label and control
```

The Gap property provides consistent spacing and is the recommended way to control label-control distance for standard positions.

### DX and DY Properties

For custom positioning or fine-tuning, use DX and DY properties:

```csharp
this.autoLabel1.DX = -70;  // Horizontal offset
this.autoLabel1.DY = 3;    // Vertical offset
this.autoLabel1.Gap = 10;  // Additional gap
```

```vb
Me.autoLabel1.DX = -70  ' Horizontal offset
Me.autoLabel1.DY = 3    ' Vertical offset
Me.autoLabel1.Gap = 10  ' Additional gap
```

## Complete Example: Multiple Positioning Styles

Here's an example showing different positioning options:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace AutoLabelPositioning
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            
            // Left-positioned label
            TextBox leftTextBox = new TextBox();
            leftTextBox.Location = new Point(150, 50);
            leftTextBox.Size = new Size(200, 20);
            
            AutoLabel leftLabel = new AutoLabel();
            leftLabel.Text = "Name (Left):";
            leftLabel.LabeledControl = leftTextBox;
            leftLabel.Position = AutoLabelPosition.Left;
            leftLabel.Gap = 10;
            
            // Top-positioned label
            TextBox topTextBox = new TextBox();
            topTextBox.Location = new Point(150, 120);
            topTextBox.Size = new Size(200, 20);
            
            AutoLabel topLabel = new AutoLabel();
            topLabel.Text = "Address (Top):";
            topLabel.LabeledControl = topTextBox;
            topLabel.Position = AutoLabelPosition.Top;
            topLabel.Gap = 5;
            
            // Custom-positioned label
            TextBox customTextBox = new TextBox();
            customTextBox.Location = new Point(150, 200);
            customTextBox.Size = new Size(200, 20);
            
            AutoLabel customLabel = new AutoLabel();
            customLabel.Text = "Custom:";
            customLabel.LabeledControl = customTextBox;
            customLabel.Position = AutoLabelPosition.Custom;
            customLabel.DX = -80;
            customLabel.DY = 5;
            
            // Add all controls
            this.Controls.Add(leftTextBox);
            this.Controls.Add(leftLabel);
            this.Controls.Add(topTextBox);
            this.Controls.Add(topLabel);
            this.Controls.Add(customTextBox);
            this.Controls.Add(customLabel);
        }
    }
}
```

## Best Practices

### 1. Choose the Right Position

- **Use Left** for standard horizontal forms where labels precede input fields
- **Use Top** for vertical forms or when controls need more horizontal space
- **Use Side** for alternative layouts or help text positioning
- **Use Custom** only when standard positions don't meet requirements

### 2. Consistent Gap Values

Use the same Gap value across all labels in a form for visual consistency:

```csharp
const int STANDARD_GAP = 10;

label1.Gap = STANDARD_GAP;
label2.Gap = STANDARD_GAP;
label3.Gap = STANDARD_GAP;
```

### 3. Avoid Overlap

Ensure your Gap and positioning values prevent the label from overlapping the control:

```csharp
// Good: Sufficient gap
autoLabel1.Gap = 10;

// Bad: May cause overlap
autoLabel1.Gap = 0;
autoLabel1.DX = 5;  // Too close
```

### 4. Test Dynamic Layouts

When controls move dynamically, verify labels follow correctly:

```csharp
// Test: Move the control programmatically
textBox1.Location = new Point(200, 150);
// The AutoLabel should automatically reposition
```

### 5. Consider FlowLayout

For dynamic forms, use FlowLayoutPanel which treats AutoLabel-control pairs as units:

```csharp
FlowLayoutPanel panel = new FlowLayoutPanel();
panel.FlowDirection = FlowDirection.TopDown;

// Add labeled controls - FlowLayout handles them as pairs
panel.Controls.Add(textBox1);
panel.Controls.Add(autoLabel1);  // Paired with textBox1
```

## Common Scenarios

### Scenario 1: Standard Form with Left Labels

```csharp
// All labels to the left with 10px gap
autoLabel1.Position = AutoLabelPosition.Left;
autoLabel1.Gap = 10;

autoLabel2.Position = AutoLabelPosition.Left;
autoLabel2.Gap = 10;
```

### Scenario 2: Vertical Stacked Form

```csharp
// Labels above controls
autoLabel1.Position = AutoLabelPosition.Top;
autoLabel1.Gap = 5;

autoLabel2.Position = AutoLabelPosition.Top;
autoLabel2.Gap = 5;
```

### Scenario 3: Custom Offset for Special Alignment

```csharp
// Fine-tune position for special alignment
autoLabel1.Position = AutoLabelPosition.Custom;
autoLabel1.DX = -120;  // Further left
autoLabel1.DY = 3;     // Slightly down for vertical centering
```

## Troubleshooting

**Issue**: Label appears in wrong position
- Check that `LabeledControl` is set correctly
- Verify `Position` property value
- Ensure control has been added to form

**Issue**: Gap not working
- Gap only works with Left, Top, and Side positions
- For Custom position, use DX and DY instead

**Issue**: Label doesn't move when control moves
- Verify `LabeledControl` property is set
- Ensure both controls are in the same container
- Check that Position is not set to Custom with fixed coordinates
