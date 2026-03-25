# Anchoring and Fill Behavior

## Anchor Property

**Purpose:** Determines how a control is positioned and justified within its allocated grid cell when the cell is larger than the control's preferred size.

**Default Value:** AnchorTypes.Center

**Valid Options:**
- Center
- North
- NorthEast
- East
- SouthEast
- South
- SouthWest
- West
- NorthWest

### Understanding Anchor

The anchor property only has an effect when:
1. The control's allocated cell is **larger** than the control's preferred size
2. The Fill property is set to **None**

When Fill is set to Both, Horizontal, or Vertical, the control resizes to fill the space, making Anchor irrelevant.

### Anchor Options Explained

```
              North
                |
    NorthWest - * - NorthEast
        |       |       |
West -- * --- Center --- * -- East
        |       |       |
    SouthWest - * - SouthEast
                |
              South
```

**North:** Top center alignment
```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.North, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
```

**NorthEast:** Top right corner
```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.NorthEast, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
```

**East:** Middle right alignment
```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.East, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
```

**SouthEast:** Bottom right corner
```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.SouthEast, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
```

**South:** Bottom center alignment
```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.South, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
```

**SouthWest:** Bottom left corner
```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.SouthWest, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
```

**West:** Middle left alignment
```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.West, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
```

**NorthWest:** Top left corner
```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.NorthWest, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
```

### Practical Example: Mixed Anchoring

```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

ButtonAdv btn1 = new ButtonAdv { Text = "NE" };
ButtonAdv btn2 = new ButtonAdv { Text = "S" };
ButtonAdv btn3 = new ButtonAdv { Text = "E" };
ButtonAdv btn4 = new ButtonAdv { Text = "NW" };

this.Controls.Add(btn1);
this.Controls.Add(btn2);
this.Controls.Add(btn3);
this.Controls.Add(btn4);

layout.SetConstraints(btn1, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.NorthEast, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn2, new GridBagConstraints(1, 0, 1, 1, 1, 1, AnchorTypes.South, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn3, new GridBagConstraints(0, 1, 1, 1, 1, 1, AnchorTypes.East, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn4, new GridBagConstraints(1, 1, 1, 1, 1, 1, AnchorTypes.NorthWest, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
```

**Result:** Four buttons positioned at different corners/edges of their cells.

## Fill Property

**Purpose:** Determines whether and how a control resizes to fill its allocated cell space.

**Default Value:** FillType.None

**Valid Options:**
- None
- Both
- Horizontal
- Vertical

### Fill Options

**None:** Control maintains its preferred size (doesn't resize)

```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
```

Result: Control stays at its natural size. Anchor property determines position within cell.

**Horizontal:** Control stretches to fill cell width but maintains height

```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Horizontal, new Insets(0, 0, 0, 0), 0, 0, false));
```

Result: Control width = cell width. Height = preferred height.

**Vertical:** Control stretches to fill cell height but maintains width

```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Vertical, new Insets(0, 0, 0, 0), 0, 0, false));
```

Result: Control height = cell height. Width = preferred width.

**Both:** Control stretches to fill entire cell (both width and height)

```csharp
layout.SetConstraints(btn, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
```

Result: Control completely fills the cell. Anchor property is ignored.

### Fill Combinations

```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

ButtonAdv btn1 = new ButtonAdv { Text = "None" };
ButtonAdv btn2 = new ButtonAdv { Text = "Horizontal" };
ButtonAdv btn3 = new ButtonAdv { Text = "Vertical" };
ButtonAdv btn4 = new ButtonAdv { Text = "Both" };

this.Controls.Add(btn1);
this.Controls.Add(btn2);
this.Controls.Add(btn3);
this.Controls.Add(btn4);

layout.SetConstraints(btn1, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn2, new GridBagConstraints(1, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Horizontal, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn3, new GridBagConstraints(0, 1, 1, 1, 1, 1, AnchorTypes.Center, FillType.Vertical, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn4, new GridBagConstraints(1, 1, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
```

**Result:** 2×2 grid demonstrating all four fill options.

## Insets Property

**Purpose:** Specifies the **minimum padding** around a control before it's laid out.

**Type:** Insets object

**Constructor:**
```csharp
new Insets(int top, int left, int bottom, int right)
```

**Default Value:** Null (no padding)

### How Insets Work

Insets add space around the control but don't increase the control's size. Instead, they add to the preferred size calculation:

- Effective preferred width = control width + left inset + right inset
- Effective preferred height = control height + top inset + bottom inset

This space is always maintained around the control, even when the cell is larger.

### Insets Example

```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

ButtonAdv btn1 = new ButtonAdv { Text = "No padding" };
ButtonAdv btn2 = new ButtonAdv { Text = "With padding" };

this.Controls.Add(btn1);
this.Controls.Add(btn2);

// No insets
layout.SetConstraints(btn1, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));

// With 5 pixels padding on all sides
layout.SetConstraints(btn2, new GridBagConstraints(1, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(5, 5, 5, 5), 0, 0, false));
```

**Result:** Button 2 has 5 pixels of space around it.

### Common Inset Patterns

```csharp
// Equal padding all sides
new Insets(5, 5, 5, 5)

// Horizontal padding only
new Insets(0, 10, 0, 10)

// Vertical padding only
new Insets(10, 0, 10, 0)

// Different values
new Insets(2, 4, 2, 4)  // top, left, bottom, right
```

## IPadX and IPadY Properties

**Purpose:** Adds to the control's **declared preferred size** when calculating layout dimensions.

**Default Value:** Null (or 0)

**Valid Range:** 0 or positive integer (pixels)

### How IPad Works

- IPadX: Added to preferred width (left + right padding)
- IPadY: Added to preferred height (top + bottom padding)

Example: If a button is 80 pixels wide and IPadX = 10:
- Preferred width for layout = 80 + 10 = 90 pixels

### IPad vs Insets Difference

| Property | Effect | Visual Result |
|----------|--------|---------------|
| **Insets** | Padding around control | Space between control and cell edge |
| **IPad** | Increases cell size | Control stays smaller in a larger cell |

### IPad Example

```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

ButtonAdv btn1 = new ButtonAdv { Text = "Normal" };
ButtonAdv btn2 = new ButtonAdv { Text = "Extra space" };

this.Controls.Add(btn1);
this.Controls.Add(btn2);

// Standard layout
layout.SetConstraints(btn1, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));

// With IPadX = 20 and IPadY = 10
layout.SetConstraints(btn2, new GridBagConstraints(1, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 20, 10, false));
```

**Result:** Button 2 is allocated extra space in its cell (20 extra pixels width, 10 extra pixels height).

## Practical Layout Patterns

### Pattern 1: Compact Button Bar (No Fill, Center Anchor)

```csharp
for (int i = 0; i < 3; i++)
{
    ButtonAdv btn = new ButtonAdv { Text = $"Button {i}" };
    this.Controls.Add(btn);
    layout.SetConstraints(btn, new GridBagConstraints(i, 0, 1, 1, 0, 0, AnchorTypes.Center, FillType.None, new Insets(2, 2, 2, 2), 0, 0, false));
}
```

Result: Three buttons at natural size, with 2 pixel padding, centered in their cells.

### Pattern 2: Full-Width Form (Fill.Horizontal)

```csharp
LabelAdv lbl = new LabelAdv { Text = "Name:" };
TextBoxExt txt = new TextBoxExt();

this.Controls.Add(lbl);
this.Controls.Add(txt);

layout.SetConstraints(lbl, new GridBagConstraints(0, 0, 1, 1, 0, 1, AnchorTypes.West, FillType.None, new Insets(5, 5, 5, 5), 0, 0, false));
layout.SetConstraints(txt, new GridBagConstraints(1, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Horizontal, new Insets(5, 5, 5, 5), 0, 0, false));
```

Result: Label stays at natural width, TextBox stretches to fill remaining horizontal space.

### Pattern 3: Uniform Grid (Fill.Both, Equal Weights)

```csharp
for (int row = 0; row < 3; row++)
{
    for (int col = 0; col < 3; col++)
    {
        ButtonAdv btn = new ButtonAdv { Text = $"R{row}C{col}" };
        this.Controls.Add(btn);
        layout.SetConstraints(btn, new GridBagConstraints(col, row, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(2, 2, 2, 2), 0, 0, false));
    }
}
```

Result: 3×3 grid where all cells grow equally, buttons fill their cells.

### Pattern 4: Corners & Edges (No Fill with Strategic Anchoring)

```csharp
ButtonAdv btnNE = new ButtonAdv { Text = "NE" };
ButtonAdv btnSW = new ButtonAdv { Text = "SW" };
ButtonAdv btnCenter = new ButtonAdv { Text = "Center" };

this.Controls.Add(btnNE);
this.Controls.Add(btnSW);
this.Controls.Add(btnCenter);

layout.SetConstraints(btnNE, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.NorthEast, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btnSW, new GridBagConstraints(1, 1, 1, 1, 1, 1, AnchorTypes.SouthWest, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btnCenter, new GridBagConstraints(0, 1, 1, 1, 1, 1, AnchorTypes.Center, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
```

Result: Three buttons positioned at specific locations within their cells using anchoring.
