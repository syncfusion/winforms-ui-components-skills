# Alignment Options

## Table of Contents
- [Alignment Property](#alignment-property)
- [Simple Alignment Modes](#simple-alignment-modes)
- [ChildConstraints-Based Alignment](#childconstraints-based-alignment)
- [Choosing Between Approaches](#choosing-between-approaches)
- [Switching Alignment at Runtime](#switching-alignment-at-runtime)
- [Complete Examples](#complete-examples)

---

## Alignment Property

The `Alignment` property determines how controls are positioned within rows (horizontal mode) or columns (vertical mode). It supports both simple and constraint-based approaches.

```csharp
flowLayout1.Alignment = FlowAlignment.Center;
```

## Simple Alignment Modes

Simple alignment modes apply uniform positioning to all controls. Choose one:

### FlowAlignment.Near

Aligns controls to the start of the flow direction:

```csharp
flowLayout1.Alignment = FlowAlignment.Near;
```

**Horizontal Mode:** Controls align left within each row
```
Near:
[Control1] [Control2] [Control3]
```

**Vertical Mode:** Controls align top within each column
```
Near:
[Control1]
[Control2]
[Control3]
```

**Use Cases:** Left-aligned button bars, top-aligned option lists

### FlowAlignment.Center

Centers controls along the flow direction:

```csharp
flowLayout1.Alignment = FlowAlignment.Center;
```

**Horizontal Mode:** Rows center horizontally
```
Center:
            [Control1] [Control2] [Control3]
```

**Vertical Mode:** Columns center vertically
```
Center:
       [Control1]
       [Control2]
       [Control3]
```

**Use Cases:** Centered button bars, centered control panels

### FlowAlignment.Far

Aligns controls to the end of the flow direction:

```csharp
flowLayout1.Alignment = FlowAlignment.Far;
```

**Horizontal Mode:** Controls align right within each row
```
Far:
                          [Control1] [Control2] [Control3]
```

**Vertical Mode:** Controls align bottom within each column
```
Far:
[Control1]
[Control2]
[Control3]
```

**Use Cases:** Right-aligned buttons (OK/Cancel), bottom-aligned controls

## ChildConstraints-Based Alignment

ChildConstraints alignment enables per-control positioning for complex layouts. This approach uses individual constraints on each child control.

```csharp
flowLayout1.Alignment = FlowAlignment.ChildConstraints;
```

### When to Use ChildConstraints

- Mixing different alignment requirements in one layout
- Stretching specific controls to fill available space
- Creating complex resizable forms
- Variable control sizing within layout

### Individual Control Alignment

With ChildConstraints mode, each control's HAlign and VAlign properties determine its position within its row/column.

**Example: Mixed Alignment**
```csharp
flowLayout1.Alignment = FlowAlignment.ChildConstraints;
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;

// Set individual alignments
var constraints1 = new FlowLayoutConstraints(
    active: true,
    hAlign: HorzFlowAlign.Left,    // Left-aligned
    vAlign: VertFlowAlign.Center,
    newLine: false,
    proportionalColWidth: false,
    proportionalRowHeight: false
);
flowLayout1.SetConstraints(control1, constraints1);

var constraints2 = new FlowLayoutConstraints(
    active: true,
    hAlign: HorzFlowAlign.Justify,  // Stretch to fill
    vAlign: VertFlowAlign.Center,
    newLine: false,
    proportionalColWidth: false,
    proportionalRowHeight: false
);
flowLayout1.SetConstraints(control2, constraints2);
```

**Result:**
```
[Control1]    [Control2_stretches_to_fill_space]
```

### Justify Alignment

The `HorzFlowAlign.Justify` option stretches controls to fill available horizontal space:

```csharp
var constraints = new FlowLayoutConstraints(
    active: true,
    hAlign: HorzFlowAlign.Justify,  // Stretch horizontally
    vAlign: VertFlowAlign.Center,
    newLine: false,
    proportionalColWidth: false,
    proportionalRowHeight: false
);

flowLayout1.SetConstraints(textBoxControl, constraints);
```

**Effect:** Control expands to fit remaining row width after other controls are sized.

**Multiple Justified Controls:** Space is proportionally distributed based on minimum and preferred sizes.

## Choosing Between Approaches

### Use Simple Alignment When:

- All controls should align the same way
- Layout is straightforward (all left, all center, or all right)
- You need quick, uniform positioning
- Simple button bars or option lists

```csharp
// Simple approach - all controls center
flowLayout1.Alignment = FlowAlignment.Center;
```

### Use ChildConstraints When:

- Different controls need different alignment
- Some controls should stretch, others shouldn't
- Building complex, resizable forms
- Creating data entry forms with labels and inputs

```csharp
// Complex approach - mixed alignment
flowLayout1.Alignment = FlowAlignment.ChildConstraints;

// Label: Left-aligned
flowLayout1.SetConstraints(label, leftConstraints);

// TextBox: Stretch to fill
flowLayout1.SetConstraints(textBox, justifyConstraints);
```

## Switching Alignment at Runtime

Change alignment properties dynamically:

```csharp
private void SetAlignmentMode(string mode)
{
    switch (mode)
    {
        case "Left":
            flowLayout1.Alignment = FlowAlignment.Near;
            break;
        case "Center":
            flowLayout1.Alignment = FlowAlignment.Center;
            break;
        case "Right":
            flowLayout1.Alignment = FlowAlignment.Far;
            break;
        case "Constrained":
            flowLayout1.Alignment = FlowAlignment.ChildConstraints;
            break;
    }
}
```

The layout automatically updates when alignment changes.

## Complete Examples

### Example 1: Simple Center-Aligned Buttons

```csharp
flowLayout1.Alignment = FlowAlignment.Center;
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.HGap = 10;
flowLayout1.VGap = 10;

// Add OK and Cancel buttons
Button okBtn = new Button { Text = "OK", Size = new Size(80, 30) };
Button cancelBtn = new Button { Text = "Cancel", Size = new Size(80, 30) };

this.Controls.Add(okBtn);
this.Controls.Add(cancelBtn);

// Result: Both buttons centered in the form
// [                  [OK] [Cancel]                  ]
```

### Example 2: Left-Aligned Button Bar

```csharp
flowLayout1.Alignment = FlowAlignment.Near;
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.HGap = 5;
flowLayout1.VGap = 5;

// Add toolbar buttons
for (int i = 1; i <= 5; i++)
{
    Button btn = new Button { Text = "Tool " + i, Size = new Size(70, 25) };
    this.Controls.Add(btn);
}

// Result: Buttons left-aligned
// [Tool1] [Tool2] [Tool3] [Tool4] [Tool5]
```

### Example 3: Right-Aligned Controls

```csharp
flowLayout1.Alignment = FlowAlignment.Far;
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.HGap = 8;

// Add buttons - they'll appear right-aligned
Button btn1 = new Button { Text = "Save", Size = new Size(80, 30) };
Button btn2 = new Button { Text = "Cancel", Size = new Size(80, 30) };

this.Controls.Add(btn1);
this.Controls.Add(btn2);

// Result: Buttons right-aligned
//                                         [Save] [Cancel]
```

### Example 4: Mixed Alignment with ChildConstraints

```csharp
using Syncfusion.Windows.Forms.Tools;

flowLayout1.Alignment = FlowAlignment.ChildConstraints;
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.HGap = 10;
flowLayout1.VGap = 10;

// Label - left-aligned
Label label = new Label 
{ 
    Text = "Search:", 
    Size = new Size(60, 20),
    AutoSize = true
};

// TextBox - stretched to fill
TextBox textBox = new TextBox 
{ 
    Size = new Size(100, 20),
    Multiline = false
};

// Button - fixed width
Button searchBtn = new Button 
{ 
    Text = "Search", 
    Size = new Size(80, 20) 
};

this.Controls.Add(label);
this.Controls.Add(textBox);
this.Controls.Add(searchBtn);

// Set constraints
flowLayout1.SetConstraints(label, new FlowLayoutConstraints(
    true, HorzFlowAlign.Left, VertFlowAlign.Center, false, false, false));

flowLayout1.SetConstraints(textBox, new FlowLayoutConstraints(
    true, HorzFlowAlign.Justify, VertFlowAlign.Center, false, false, false));

flowLayout1.SetConstraints(searchBtn, new FlowLayoutConstraints(
    true, HorzFlowAlign.Left, VertFlowAlign.Center, false, false, false));

// Result:
// [Search:] [TextBox__________________________] [Search]
```

### Example 5: Vertical Alignment

```csharp
flowLayout1.Alignment = FlowAlignment.Center;
flowLayout1.LayoutMode = FlowLayoutMode.Vertical;
flowLayout1.HGap = 15;
flowLayout1.VGap = 5;

// Add options
for (int i = 1; i <= 4; i++)
{
    CheckBox chk = new CheckBox { Text = "Option " + i, Size = new Size(100, 20) };
    this.Controls.Add(chk);
}

// Result: Options centered in a column layout
//                [Option 1]
//                [Option 2]
//                [Option 3]
//                [Option 4]
```

## Tips

1. **Start simple:** Use Near, Center, or Far for most layouts
2. **Graduate to ChildConstraints:** Only use when simple alignment doesn't work
3. **Test responsive:** Resize the form to verify alignment at different sizes
4. **Combine with spacing:** Use HGap and VGap with alignment for professional appearance
5. **Document constraints:** Document which controls use custom constraints for maintainability
