# Sizing and Space Distribution

## Table of Contents
- [Overview](#overview)
- [WeightX Property](#weightx-property)
- [WeightY Property](#weighty-property)
- [How Weight Works](#how-weight-works)
- [Weight Distribution Examples](#weight-distribution-examples)
- [GetLayoutWeights Method](#getlayoutweights-method)
- [Common Spacing Patterns](#common-spacing-patterns)

## Overview

After GridBagLayout determines the base size of rows and columns from control preferred sizes, it distributes any **extra available space** based on weight properties. Weights control how responsive your layout is to container resizing.

**Key Concept:** Weights don't make controls larger by default—they determine how extra space is allocated when the container is larger than needed.

## WeightX Property

**Purpose:** Controls how a control's column participates in distributing extra horizontal space.

**Default Value:** Null (or 0)

**Valid Range:** 0 to any positive number (double)

**Behavior:**
- If WeightX is 0 or Null: Column doesn't receive extra horizontal space
- If WeightX > 0: Column receives share of extra space proportional to its weight
- If multiple controls in a column have different WeightX, the column uses the highest value

### Example: No Horizontal Distribution (Default)

```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

ButtonAdv btn1 = new ButtonAdv { Text = "Btn1" };
ButtonAdv btn2 = new ButtonAdv { Text = "Btn2" };

this.Controls.Add(btn1);
this.Controls.Add(btn2);

// Both with WeightX = 0 (or Null by default)
layout.SetConstraints(btn1, new GridBagConstraints(0, 0, 1, 1, 0, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn2, new GridBagConstraints(1, 0, 1, 1, 0, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
```

**Result:** Columns stay at their preferred width. Extra horizontal space is not distributed; the grid stays centered with unused space on sides.

### Example: Equal Horizontal Distribution

```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

ButtonAdv btn1 = new ButtonAdv { Text = "Btn1" };
ButtonAdv btn2 = new ButtonAdv { Text = "Btn2" };

this.Controls.Add(btn1);
this.Controls.Add(btn2);

// Both with WeightX = 1
layout.SetConstraints(btn1, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn2, new GridBagConstraints(1, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
```

**Result:** Extra horizontal space is divided equally between columns 0 and 1. Both columns grow equally when the form is resized.

## WeightY Property

**Purpose:** Controls how a control's row participates in distributing extra vertical space.

**Default Value:** Null (or 0)

**Valid Range:** 0 to any positive number (double)

**Behavior:**
- If WeightY is 0 or Null: Row doesn't receive extra vertical space
- If WeightY > 0: Row receives share of extra space proportional to its weight
- If multiple controls in a row have different WeightY, the row uses the highest value

### Example: No Vertical Distribution (Default)

```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

ButtonAdv btn1 = new ButtonAdv { Text = "Btn1" };
ButtonAdv btn2 = new ButtonAdv { Text = "Btn2" };

this.Controls.Add(btn1);
this.Controls.Add(btn2);

// Both with WeightY = 0 (or Null by default)
layout.SetConstraints(btn1, new GridBagConstraints(0, 0, 1, 1, 1, 0, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn2, new GridBagConstraints(0, 1, 1, 1, 1, 0, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
```

**Result:** Rows stay at their preferred height. Extra vertical space is not distributed; the grid stays centered with unused space above/below.

### Example: Equal Vertical Distribution

```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

ButtonAdv btn1 = new ButtonAdv { Text = "Btn1" };
ButtonAdv btn2 = new ButtonAdv { Text = "Btn2" };

this.Controls.Add(btn1);
this.Controls.Add(btn2);

// Both with WeightY = 1
layout.SetConstraints(btn1, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn2, new GridBagConstraints(0, 1, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
```

**Result:** Extra vertical space is divided equally between rows 0 and 1. Both rows grow equally when the form is resized.

## How Weight Works

### Weight-to-Space Mapping

Weights are **ratios**, not percentages. The actual space each row/column gets is based on the ratio of its weight to the sum of all weights.

**Formula:**
```
Space for column/row = (Weight of column/row / Sum of all weights) × Extra space available
```

### Example: Proportional Distribution

```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

ButtonAdv btn1 = new ButtonAdv { Text = "Btn1" };  // WeightX = 2
ButtonAdv btn2 = new ButtonAdv { Text = "Btn2" };  // WeightX = 1

this.Controls.Add(btn1);
this.Controls.Add(btn2);

layout.SetConstraints(btn1, new GridBagConstraints(0, 0, 1, 1, 2, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn2, new GridBagConstraints(1, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
```

**Weight Ratio:** 2:1

**Result:** 
- Total weight = 2 + 1 = 3
- Column 0 gets: (2/3) of extra space
- Column 1 gets: (1/3) of extra space
- Column 0 grows twice as fast as Column 1 when form is resized

## Weight Distribution Examples

### Example 1: Three-Column Layout with Different Weights

```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

ButtonAdv btn1 = new ButtonAdv { Text = "Small" };
ButtonAdv btn2 = new ButtonAdv { Text = "Medium" };
ButtonAdv btn3 = new ButtonAdv { Text = "Large" };

this.Controls.Add(btn1);
this.Controls.Add(btn2);
this.Controls.Add(btn3);

//                           X   Y  CSpanX CSpanY WeightX WeightY
layout.SetConstraints(btn1, new GridBagConstraints(0, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn2, new GridBagConstraints(1, 0, 1, 1, 2, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn3, new GridBagConstraints(2, 0, 1, 1, 3, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
```

**Weight Ratio:** 1:2:3

**Result:** 
- When form expands, columns grow in ratio 1:2:3
- Column 2 grows fastest, Column 0 grows slowest

### Example 2: Two-Row Layout with Responsive Vertical Distribution

```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

ButtonAdv btn1 = new ButtonAdv { Text = "Top" };
ButtonAdv btn2 = new ButtonAdv { Text = "Bottom" };

this.Controls.Add(btn1);
this.Controls.Add(btn2);

layout.SetConstraints(btn1, new GridBagConstraints(0, 0, 1, 1, 1, 2, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
layout.SetConstraints(btn2, new GridBagConstraints(0, 1, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
```

**Weight Ratio:** 2:1 (vertical)

**Result:** 
- Row 0 gets 2/3 of extra vertical space
- Row 1 gets 1/3 of extra vertical space
- Top button grows twice as fast as bottom button

### Example 3: All Equal (Responsive Fill)

```csharp
for (int i = 0; i < 6; i++)
{
    ButtonAdv btn = new ButtonAdv { Text = $"Button {i}" };
    this.Controls.Add(btn);
    
    int row = i / 3;
    int col = i % 3;
    
    layout.SetConstraints(btn, new GridBagConstraints(col, row, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
}
```

**Result:** 2×3 grid where all rows and columns grow equally, creating a responsive grid that fills the entire form.

## GetLayoutWeights Method

**Purpose:** Retrieve the calculated row and column weights of the current layout.

**Syntax:**
```csharp
public Hashtable GetLayoutWeights()
```

**Returns:** Hashtable with layout weight information

**Usage:**
```csharp
GridBagLayout layout = new GridBagLayout();
layout.ContainerControl = this;

// Add controls...

Hashtable weights = layout.GetLayoutWeights();
// Now weights contains row and column weight information
```

This method is useful for debugging layouts or dynamically adjusting constraints based on current weight distribution.

## Common Spacing Patterns

### Pattern 1: Fixed Container, Centered Grid

```csharp
// All weights = 0 (default)
for (int i = 0; i < 4; i++)
{
    ButtonAdv btn = new ButtonAdv { Text = $"Button {i}" };
    this.Controls.Add(btn);
    
    int row = i / 2;
    int col = i % 2;
    
    layout.SetConstraints(btn, new GridBagConstraints(col, row, 1, 1, 0, 0, AnchorTypes.Center, FillType.None, new Insets(0, 0, 0, 0), 0, 0, false));
}
```

**Result:** Grid maintains preferred size and stays centered in the form.

### Pattern 2: Responsive Fill

```csharp
// All weights = 1 (positive value)
for (int i = 0; i < 4; i++)
{
    ButtonAdv btn = new ButtonAdv { Text = $"Button {i}" };
    this.Controls.Add(btn);
    
    int row = i / 2;
    int col = i % 2;
    
    layout.SetConstraints(btn, new GridBagConstraints(col, row, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
}
```

**Result:** Grid expands to fill entire form, with rows and columns growing equally.

### Pattern 3: Hybrid - Fixed Sidebar, Responsive Content

```csharp
// Sidebar column
ButtonAdv sidebar = new ButtonAdv { Text = "Menu" };
this.Controls.Add(sidebar);
layout.SetConstraints(sidebar, new GridBagConstraints(0, 0, 1, 1, 0, 1, AnchorTypes.Center, FillType.Vertical, new Insets(0, 0, 0, 0), 0, 0, false));

// Content column
ButtonAdv content = new ButtonAdv { Text = "Content" };
this.Controls.Add(content);
layout.SetConstraints(content, new GridBagConstraints(1, 0, 1, 1, 1, 1, AnchorTypes.Center, FillType.Both, new Insets(0, 0, 0, 0), 0, 0, false));
```

**Result:** 
- Column 0 (sidebar) stays at preferred width
- Column 1 (content) grows to fill extra horizontal space
- Both rows grow equally vertically
