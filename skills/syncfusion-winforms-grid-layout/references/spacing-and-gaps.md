# Spacing and Gaps

## Table of Contents
- [Overview](#overview)
- [Horizontal Gap (HGap)](#horizontal-gap-hgap)
- [Vertical Gap (VGap)](#vertical-gap-vgap)
- [Gap Calculations](#gap-calculations)
- [Common Spacing Patterns](#common-spacing-patterns)

## Overview

GridLayout provides two properties to control the spacing between child controls and the layout borders:

| Property | Purpose |
|----------|---------|
| **HGap** | Horizontal spacing between controls and layout border |
| **VGap** | Vertical spacing between controls and layout border |

These properties allow you to create uniform spacing throughout the grid layout without manually adjusting individual control positions.

## Horizontal Gap (HGap)

The `HGap` property specifies the horizontal distance in pixels between child controls and the layout's left and right borders.

**Property Details:**
- **Type:** Integer
- **Unit:** Pixels
- **Default:** 0
- **Range:** 0 to any positive integer

**Effect:**
- Adds space on the left side of the leftmost controls
- Adds space on the right side of the rightmost controls
- Does not directly affect spacing between controls horizontally (that's determined by column division)

**Setting HGap in Code:**

C#:
```csharp
this.gridLayout1.HGap = 10;
```

VB.NET:
```vb
Me.gridLayout1.HGap = 10
```

**Example: HGap Visualization**

With a 2-column grid and HGap = 10:
```
|--10px--|[Control 1]|[Control 2]|--10px--|
Left        Column 1     Column 2      Right
Margin                                  Margin
```

Each column expands to fill available space equally, with 10-pixel margins on both sides.

## Vertical Gap (VGap)

The `VGap` property specifies the vertical distance in pixels between child controls and the layout's top and bottom borders.

**Property Details:**
- **Type:** Integer
- **Unit:** Pixels
- **Default:** 0
- **Range:** 0 to any positive integer

**Effect:**
- Adds space above the topmost controls
- Adds space below the bottommost controls
- Does not directly affect spacing between controls vertically (that's determined by row division)

**Setting VGap in Code:**

C#:
```csharp
this.gridLayout1.VGap = 10;
```

VB.NET:
```vb
Me.gridLayout1.VGap = 10
```

**Example: VGap Visualization**

With a 2-row grid and VGap = 10:
```
|--10px--|
[Control 1]
[Control 2]
|--10px--|
Top        
Margin     Bottom Margin
```

Each row expands to fill available space equally, with 10-pixel margins on top and bottom.

## Gap Calculations

Understanding how gaps interact with the grid:

**Space Allocation:**
1. Total available width = Container width - (HGap × 2)
2. Column width = (Total available width) / (Number of columns)
3. Total available height = Container height - (VGap × 2)
4. Row height = (Total available height) / (Number of rows)

**Example Calculation:**

Given:
- Form width: 400 pixels
- Form height: 300 pixels
- Grid layout: 2 columns × 2 rows
- HGap: 10 pixels
- VGap: 10 pixels

Calculations:
```
Horizontal:
  Available width = 400 - (10 × 2) = 380
  Column width = 380 / 2 = 190 pixels

Vertical:
  Available height = 300 - (10 × 2) = 280
  Row height = 280 / 2 = 140 pixels

Result:
  |--10--|[190 px]|[190 px]|--10--|
  +------+--------+--------+------+
  |      |[140 px]|[140 px]|      |
  |      +--------+--------+      |
  |      |[140 px]|[140 px]|      |
  +------+--------+--------+------+
  |      |[140 px]|[140 px]|      |
  +------+--------+--------+------+
```

## Common Spacing Patterns

### Pattern 1: Minimal Spacing

For compact layouts with minimal margins:

C#:
```csharp
gridLayout1.HGap = 0;
gridLayout1.VGap = 0;
```

**Use Case:** Maximizing control size, compact form design

### Pattern 2: Standard Spacing (Recommended)

For balanced, professional appearance:

C#:
```csharp
gridLayout1.HGap = 5;
gridLayout1.VGap = 5;
```

**Use Case:** Most applications, consistent UI presentation

### Pattern 3: Generous Spacing

For spacious, modern layouts:

C#:
```csharp
gridLayout1.HGap = 15;
gridLayout1.VGap = 15;
```

**Use Case:** Large forms, accessibility focus, high-resolution displays

### Pattern 4: Asymmetric Spacing

Different horizontal and vertical spacing:

C#:
```csharp
gridLayout1.HGap = 10;  // Generous horizontal margins
gridLayout1.VGap = 5;   // Compact vertical spacing
```

**Use Case:** Wide forms with limited vertical space, panoramic layouts

### Pattern 5: Dynamic Spacing Based on Container Size

Adjusting gaps at runtime based on form size:

C#:
```csharp
private void Form_Resize(object sender, EventArgs e)
{
    if (this.Width > 800)
    {
        gridLayout1.HGap = 20;
        gridLayout1.VGap = 20;
    }
    else
    {
        gridLayout1.HGap = 5;
        gridLayout1.VGap = 5;
    }
}
```

**Use Case:** Responsive layouts that adapt to window size

### Pattern 6: Dense Grid Layout

Minimal spacing for information-dense interfaces:

C#:
```csharp
gridLayout1.Rows = 5;
gridLayout1.Columns = 5;
gridLayout1.HGap = 2;
gridLayout1.VGap = 2;
```

**Use Case:** Data entry forms, preference dialogs, property grids

### Pattern 7: Full-Coverage Layout

Gaps set to maximize control area:

C#:
```csharp
gridLayout1.HGap = 0;
gridLayout1.VGap = 0;
// Controls will expand to fill entire form
```

**Use Case:** Maximizing screen real estate, embedded systems

## Setting Gap Values Programmatically

**Complete Setup Example:**

C#:
```csharp
GridLayout gridLayout1 = new GridLayout();
gridLayout1.ContainerControl = this;

// Configure grid structure
gridLayout1.Rows = 3;
gridLayout1.Columns = 2;

// Apply spacing
gridLayout1.HGap = 10;
gridLayout1.VGap = 10;

// Add controls
for (int i = 0; i < 6; i++)
{
    ButtonAdv btn = new ButtonAdv() { Text = $"Button {i + 1}" };
    this.Controls.Add(btn);
    gridLayout1.SetParticipateInLayout(btn, true);
}
```

VB.NET:
```vb
Dim gridLayout1 As GridLayout = New GridLayout()
gridLayout1.ContainerControl = Me

' Configure grid structure
gridLayout1.Rows = 3
gridLayout1.Columns = 2

' Apply spacing
gridLayout1.HGap = 10
gridLayout1.VGap = 10

' Add controls
For i As Integer = 0 To 5
    Dim btn As ButtonAdv = New ButtonAdv() With {.Text = $"Button {i + 1}"}
    Me.Controls.Add(btn)
    gridLayout1.SetParticipateInLayout(btn, True)
Next
```

**Changing Gaps at Runtime:**

C#:
```csharp
// Initial spacing
gridLayout1.HGap = 5;
gridLayout1.VGap = 5;

// User toggles spacing mode
private void buttonToggleSpacing_Click(object sender, EventArgs e)
{
    if (gridLayout1.HGap == 5)
    {
        gridLayout1.HGap = 20;
        gridLayout1.VGap = 20;
    }
    else
    {
        gridLayout1.HGap = 5;
        gridLayout1.VGap = 5;
    }
}
```

**Key Points:**
- Both HGap and VGap are applied simultaneously
- Gaps affect the entire layout, not individual controls
- Changing gap values at runtime immediately recalculates layout
- Gaps are symmetric (equal on both sides)
