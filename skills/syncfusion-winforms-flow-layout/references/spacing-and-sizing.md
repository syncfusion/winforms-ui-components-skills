# Spacing and Sizing

## HGap Property

The `HGap` property sets the horizontal spacing between controls:

```csharp
flowLayout1.HGap = 10;  // 10 pixels
```

### Horizontal Layout (LayoutMode.Horizontal)

In horizontal mode, HGap controls the spacing between controls within a row:

```
HGap = 5:
[Button1][Button2][Button3]  ← 5 pixels between each

HGap = 15:
[Button1]          [Button2]          [Button3]  ← 15 pixels between each
```

### Vertical Layout (LayoutMode.Vertical)

In vertical layout, HGap controls the spacing between columns:

```
HGap = 10:
[Opt1]          [Opt4]
[Opt2]          [Opt5]
[Opt3]          [Opt6]
↑ 10 pixels ↑
```

### Default Value

The default HGap value is **0** (no spacing).

### Practical Values

- **Compact:** 0-5 pixels
- **Normal:** 5-15 pixels
- **Spacious:** 15-25 pixels

## VGap Property

The `VGap` property sets the vertical spacing between controls:

```csharp
flowLayout1.VGap = 10;  // 10 pixels
```

### Horizontal Layout (LayoutMode.Horizontal)

In horizontal mode, VGap controls the spacing between rows:

```
VGap = 5:
[Btn1] [Btn2] [Btn3]
                        ↑ 5 pixels ↓
[Btn4] [Btn5] [Btn6]
```

### Vertical Layout (LayoutMode.Vertical)

In vertical layout, VGap controls the spacing between controls within a column:

```
VGap = 8:
[Option1]
        ↑ 8 pixels ↓
[Option2]
        ↑ 8 pixels ↓
[Option3]
```

### Default Value

The default VGap value is **0** (no spacing).

## Setting Gaps

### In Designer

1. Select the FlowLayout component
2. In the Properties window, locate **HGap** and **VGap**
3. Enter desired pixel values

### Programmatically

```csharp
flowLayout1.HGap = 12;
flowLayout1.VGap = 8;
```

### Independent Gaps

HGap and VGap are independent. You can set different values:

```csharp
// Tight horizontal, loose vertical
flowLayout1.HGap = 3;
flowLayout1.VGap = 15;

// Loose horizontal, tight vertical
flowLayout1.HGap = 20;
flowLayout1.VGap = 2;
```

## AutoHeight Property

The `AutoHeight` property automatically adjusts the container height based on content in horizontal layout mode:

```csharp
flowLayout1.AutoHeight = true;
```

### Behavior

- **AutoHeight = true:** Container height expands to fit all rows of controls
- **AutoHeight = false:** Container height remains fixed; controls may overflow

### Horizontal Mode Example

```csharp
// Initial: Form height = 200 pixels, fits 2 rows of buttons
// When resizing form narrower, more rows needed

flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.AutoHeight = true;  // Form height auto-expands

// If 6 buttons now wrap to 4 rows:
// Form height automatically increases to accommodate all 4 rows
```

### Use Cases

**Responsive Dialogs:**
```csharp
// Dialog auto-expands when controls need more rows
flowLayout1.AutoHeight = true;
```

**Dynamic Content:**
```csharp
// When adding controls programmatically
flowLayout1.AutoHeight = true;
// Container automatically grows to fit new controls
```

### Vertical Mode

AutoHeight has no effect in vertical mode. Use the container's width instead.

## Container Overflow Behavior

### With AutoHeight = true (Horizontal Mode)

When controls don't fit in available width:

1. FlowLayout automatically moves controls to the next row
2. Container height expands to accommodate additional rows
3. Layout adapts dynamically to container resizing

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.AutoHeight = true;

// Result: Form grows taller as needed
```

### Without AutoHeight (Horizontal Mode)

Controls may overflow beyond container boundaries:

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.AutoHeight = false;

// Result: Controls may extend beyond visible form area
```

### Vertical Mode

Width adjusts based on the widest control in each column:

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Vertical;

// Container width expands to fit all columns
```

## Complete Example: Spacing and Sizing

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class SpacingForm : Form
{
    private FlowLayout flowLayout1;
    
    public SpacingForm()
    {
        InitializeComponent();
        
        // Setup FlowLayout
        flowLayout1 = new FlowLayout();
        flowLayout1.ContainerControl = this;
        flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
        
        // Configure spacing
        flowLayout1.HGap = 10;  // Horizontal spacing
        flowLayout1.VGap = 10;  // Vertical spacing
        flowLayout1.AutoHeight = true;  // Auto-expand vertically
        
        // Add controls
        for (int i = 1; i <= 10; i++)
        {
            Button btn = new Button
            {
                Text = "Item " + i,
                Size = new Size(80, 30)
            };
            this.Controls.Add(btn);
        }
        
        // Allow form resizing to see auto-layout
        this.AutoScaleMode = AutoScaleMode.Font;
        this.Size = new Size(400, 300);
    }
}
```

**Result:** 
- Initially displays ~4 items per row with 10px horizontal gaps
- When resizing form narrower, items wrap to additional rows
- Form height auto-adjusts to fit all rows (10px vertical gaps between rows)

## Responsive Design Patterns

### Compact Spacing (Toolbars)
```csharp
flowLayout1.HGap = 2;
flowLayout1.VGap = 2;
flowLayout1.AutoHeight = true;
```

### Normal Spacing (Control Panels)
```csharp
flowLayout1.HGap = 8;
flowLayout1.VGap = 8;
flowLayout1.AutoHeight = true;
```

### Spacious Layout (Configuration Forms)
```csharp
flowLayout1.HGap = 15;
flowLayout1.VGap = 12;
flowLayout1.AutoHeight = true;
```

### Asymmetric Spacing
```csharp
// More space horizontally, less vertically
flowLayout1.HGap = 20;
flowLayout1.VGap = 5;
```

## Tips

1. **Start with moderate values:** 8-10 pixels typically provides good visual balance
2. **Test with different container sizes:** Resize to verify layout behavior
3. **Consider control sizes:** Smaller controls may need tighter spacing
4. **Use AutoHeight in horizontal mode:** Prevents controls from overlapping
5. **Adjust for content:** Forms with varied control sizes may benefit from asymmetric gaps
