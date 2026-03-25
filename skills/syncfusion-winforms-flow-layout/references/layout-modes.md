# Layout Modes and Direction

## LayoutMode Property

The `LayoutMode` property determines whether child controls flow horizontally or vertically.

### Horizontal Layout (Default)

Arranges controls left-to-right, wrapping to the next row when space is exhausted:

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
```

**Behavior:**
- Controls flow from left to right
- When a row is full, wrapping occurs to the next row
- Row height is determined by the tallest control in that row
- Additional rows stack downward

**Use Cases:**
- Button bars and toolbars
- Horizontal control sequences
- Tag clouds or keyword lists

**Example:**
```csharp
// Create 6 buttons
for (int i = 1; i <= 6; i++)
{
    Button btn = new Button { Text = "Btn" + i, Size = new Size(60, 30) };
    this.Controls.Add(btn);
}

flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.HGap = 5;
flowLayout1.VGap = 5;

// Result: If form width fits 4 buttons, layout is:
// [Btn1] [Btn2] [Btn3] [Btn4]
// [Btn5] [Btn6]
```

### Vertical Layout

Arranges controls top-to-bottom, wrapping to the next column when space is exhausted:

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Vertical;
```

**Behavior:**
- Controls flow from top to bottom
- When a column is full, wrapping occurs to the next column (right)
- Column width is determined by the widest control in that column
- Additional columns extend rightward

**Use Cases:**
- Vertical option lists (checkboxes, radio buttons)
- Menu-like vertical control sequences
- Vertical option panels

**Example:**
```csharp
// Create 6 checkboxes
for (int i = 1; i <= 6; i++)
{
    CheckBox chk = new CheckBox { Text = "Option " + i, Size = new Size(100, 20) };
    this.Controls.Add(chk);
}

flowLayout1.LayoutMode = FlowLayoutMode.Vertical;
flowLayout1.HGap = 10;
flowLayout1.VGap = 5;

// Result: If form height fits 3 checkboxes per column, layout is:
// [Option 1]  [Option 4]
// [Option 2]  [Option 5]
// [Option 3]  [Option 6]
```

## ReverseRows Property

The `ReverseRows` property reverses the direction of flow, enabling right-to-left or bottom-to-top layouts.

### Horizontal Mode with ReverseRows

Reverses horizontal flow to right-to-left:

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.ReverseRows = true;
```

**Effect:** Controls flow from right to left instead of left to right.

```
// Normal (ReverseRows = false):
[Btn1] [Btn2] [Btn3]

// Reversed (ReverseRows = true):
[Btn3] [Btn2] [Btn1]
```

### Vertical Mode with ReverseRows

Reverses vertical flow to bottom-to-top:

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Vertical;
flowLayout1.ReverseRows = true;
```

**Effect:** Controls flow from bottom to top instead of top to bottom.

### Switching Layout Mode at Runtime

Change the layout mode dynamically based on user action:

```csharp
private void ToggleLayoutMode()
{
    if (flowLayout1.LayoutMode == FlowLayoutMode.Horizontal)
    {
        flowLayout1.LayoutMode = FlowLayoutMode.Vertical;
    }
    else
    {
        flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
    }
}
```

The container automatically re-lays out child controls when the mode changes.

## ParticipateInLayout

Control whether specific child controls are included in the layout:

### SetParticipateInLayout Method

Dynamically add or remove a control from the layout:

```csharp
// Exclude button from layout
flowLayout1.SetParticipateInLayout(button1, false);

// Include button in layout
flowLayout1.SetParticipateInLayout(button1, true);
```

**VB.NET:**
```vb
' Exclude button from layout
flowLayout1.SetParticipateInLayout(button1, False)

' Include button in layout
flowLayout1.SetParticipateInLayout(button1, True)
```

### GetParticipateInLayout Method

Check if a control participates in the layout:

```csharp
bool participates = flowLayout1.GetParticipateInLayout(button1);

if (!participates)
{
    flowLayout1.SetParticipateInLayout(button1, true);
}
```

### Use Cases

**Conditional Control Visibility:**
```csharp
// Show/hide advanced options
flowLayout1.SetParticipateInLayout(advancedPanel, showAdvanced);
```

**Dynamic Option Panels:**
```csharp
// Remove controls that aren't applicable
foreach (Control control in controls)
{
    if (!IsApplicableControl(control))
    {
        flowLayout1.SetParticipateInLayout(control, false);
    }
}
```

**Performance Optimization:**
```csharp
// Temporarily exclude controls during batch operations
foreach (Control control in nonEssentialControls)
{
    flowLayout1.SetParticipateInLayout(control, false);
}
// Perform updates
foreach (Control control in nonEssentialControls)
{
    flowLayout1.SetParticipateInLayout(control, true);
}
```

## Complete Example: Switchable Layout

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class SwitchableLayoutForm : Form
{
    private FlowLayout flowLayout1;
    private Button layoutToggle;
    
    public SwitchableLayoutForm()
    {
        InitializeComponent();
        
        // Setup FlowLayout
        flowLayout1 = new FlowLayout();
        flowLayout1.ContainerControl = this;
        flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
        flowLayout1.HGap = 10;
        flowLayout1.VGap = 10;
        
        // Create toggle button
        layoutToggle = new Button
        {
            Text = "Toggle Layout",
            Size = new Size(100, 30),
            Dock = DockStyle.Top
        };
        layoutToggle.Click += (s, e) => ToggleLayout();
        this.Controls.Add(layoutToggle);
        
        // Add content controls
        for (int i = 1; i <= 6; i++)
        {
            Button btn = new Button
            {
                Text = "Control " + i,
                Size = new Size(80, 30)
            };
            this.Controls.Add(btn);
        }
    }
    
    private void ToggleLayout()
    {
        if (flowLayout1.LayoutMode == FlowLayoutMode.Horizontal)
        {
            flowLayout1.LayoutMode = FlowLayoutMode.Vertical;
            layoutToggle.Text = "Switch to Horizontal";
        }
        else
        {
            flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
            layoutToggle.Text = "Switch to Vertical";
        }
    }
}
```

## Common Patterns

### Button Bar (Horizontal)
```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.HGap = 5;
flowLayout1.VGap = 5;
flowLayout1.ReverseRows = false;
```

### Option List (Vertical)
```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Vertical;
flowLayout1.HGap = 10;
flowLayout1.VGap = 3;
flowLayout1.ReverseRows = false;
```

### Right-to-Left UI
```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.ReverseRows = true;
```

### Bottom-to-Top Layout
```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Vertical;
flowLayout1.ReverseRows = true;
```
