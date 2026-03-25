# GridBagLayout

## Table of Contents
- [Overview](#overview)
- [What is GridBagLayout](#what-is-gridbaglayout)
- [Key Features](#key-features)
- [Virtual Grid Concept](#virtual-grid-concept)
- [GridBagConstraints Class](#gridbagconstraints-class)
- [Grid Position (GridPosX, GridPosY)](#grid-position-gridposx-gridposy)
- [Weight Properties (WeightX, WeightY)](#weight-properties-weightx-weighty)
- [Cell Spanning (CellSpanX, CellSpanY)](#cell-spanning-cellspanx-cellspany)
- [Anchor Property](#anchor-property)
- [Fill Property](#fill-property)
- [Internal Padding (IPadX, IPadY)](#internal-padding-ipadx-ipady)
- [Insets (Cell Padding)](#insets-cell-padding)
- [Adding Controls via Designer](#adding-controls-via-designer)
- [Adding Controls via Code](#adding-controls-via-code)
- [Complete Examples](#complete-examples)
- [Advanced Spanning Scenarios](#advanced-spanning-scenarios)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

**GridBagLayout** is the most powerful and flexible layout manager in the package. It arranges child controls in a virtual grid where rows and columns can have variable sizes, and controls can span multiple cells. This layout manager is ideal for complex forms, dialog boxes, and any layout requiring fine-grained positioning control.

## What is GridBagLayout

**Purpose:** Create complex layouts with a flexible grid system

**Key Characteristics:**
- **Virtual grid:** Not a fixed grid like GridLayout
- **Variable cell sizes:** Each row/column can have different dimensions
- **Control spanning:** Controls can span multiple rows and/or columns
- **Constraint-based:** Each control has a GridBagConstraints object defining its position and behavior
- **Resize distribution:** WeightX/WeightY control how extra space is distributed

**Common Uses:**
- Data entry forms (labels + fields)
- Dialog boxes with complex layouts
- Settings panels
- Login/registration forms
- Calculator layouts with variable button sizes
- Any layout requiring precise control over positioning and sizing

**Also used internally by:**
- Syncfusion WizardControl (navigation buttons)
- Syncfusion Calculator control (button layout)

## Key Features

- **GridBagConstraints:** Per-control positioning with fine-grained control
- **Anchor:** 9 anchor positions (North, South, East, West, Center, NorthEast, NorthWest, SouthEast, SouthWest)
- **Fill:** None, Horizontal, Vertical, Both (resize control to fill cell)
- **CellSpan:** Row and column spanning (multi-cell controls)
- **WeightX/WeightY:** Resize distribution (0-1 scale, controls extra space allocation)
- **Insets:** Per-cell padding (space around control within cell)
- **IPadX/IPadY:** Internal padding (added to control's preferred size)

## Virtual Grid Concept

Unlike GridLayout (fixed, equal-sized cells), GridBagLayout uses a **virtual grid** where:

- **Row/column sizes** are determined by content and constraints
- **Variable dimensions:** Each row can have different height, each column different width
- **Derived from constraints:** Number of rows/columns is calculated from GridPosX/GridPosY values
- **Flexible and adaptive:** Grid adjusts to control sizes and container size

**Example:**
```
If controls are at positions (0,0), (1,0), (0,1), (1,1):
→ Results in 2x2 virtual grid

If controls are at positions (0,0), (2,0), (0,2):
→ Results in 3x3 virtual grid (even if some cells are empty)
```

## GridBagConstraints Class

Every child control in GridBagLayout has an associated **GridBagConstraints** object that defines its position, size, and behavior.

### Constructor

```csharp
GridBagConstraints(
    int gridPosX,      // Column position (0-based)
    int gridPosY,      // Row position (0-based)
    int cellSpanX,     // Columns to span
    int cellSpanY,     // Rows to span
    double weightX,    // Horizontal resize weight (0-1)
    double weightY,    // Vertical resize weight (0-1)
    AnchorTypes anchor, // Anchor position
    FillType fill,     // Fill mode
    Insets insets,     // Padding around control
    int iPadX,         // Internal horizontal padding
    int iPadY,         // Internal vertical padding
    bool isEmpty       // Usually false
)
```

### Creating Constraints

```csharp
// Full constructor
GridBagConstraints constraints = new GridBagConstraints(
    0, 0,           // Position: column 0, row 0
    1, 1,           // Span: 1 column, 1 row
    1.0, 0.0,       // Weight: take horizontal space, no vertical
    AnchorTypes.Center,
    FillType.Horizontal,
    new Insets(5, 5, 5, 5), // 5px padding on all sides
    0, 0,           // No internal padding
    false
);

// Simpler version (common defaults)
GridBagConstraints constraints = new GridBagConstraints(0, 0, 1, 1);
constraints.WeightX = 1.0;
constraints.Fill = FillType.Horizontal;
constraints.Insets = new Insets(5, 5, 5, 5);
```

## Grid Position (GridPosX, GridPosY)

### Purpose

Specify which cell (column, row) the control occupies in the virtual grid.

### Properties

| Property | Description | Default |
|----------|-------------|---------|
| **GridPosX** | Column position (0-based) | -1 |
| **GridPosY** | Row position (0-based) | -1 |

### Usage

```csharp
// Place control at column 0, row 0 (top-left)
GridBagConstraints constraints = new GridBagConstraints(0, 0, 1, 1);

// Place control at column 1, row 2
constraints.GridPosX = 1;
constraints.GridPosY = 2;
```

### Multiple Controls in Same Cell

**Note:** Multiple controls can occupy the same cell (potentially overlapping). This is usually unintentional, so ensure unique positions unless overlap is desired.

## Weight Properties (WeightX, WeightY)

### Purpose

Control how extra horizontal and vertical space is distributed when the container is larger than the virtual grid.

### Properties

| Property | Description | Default | Range |
|----------|-------------|---------|-------|
| **WeightX** | Horizontal resize weight | null (0) | 0.0 - 1.0 |
| **WeightY** | Vertical resize weight | null (0) | 0.0 - 1.0 |

### Behavior

- **0.0 (or null):** Column/row does NOT receive extra space
- **1.0:** Column/row receives proportional share of extra space
- **Relative weights:** If col1=2.0 and col2=1.0, col1 gets 2x the extra space

### Row/Column Weight Calculation

The weight of a row or column is the **largest weight** of all controls in that row/column.

**Example:**
```
Column 0 contains: control1 (WeightX=1.0), control2 (WeightX=0.5)
→ Column 0 weight = 1.0 (largest)

Column 1 contains: control3 (WeightX=2.0)
→ Column 1 weight = 2.0

Extra horizontal space distributed 1:2 between columns.
```

### Default (No Weights)

If all weights are 0 or null, the virtual grid is simply centered in the container - no extra space is distributed.

### Usage Example

```csharp
// Control takes horizontal extra space
constraints.WeightX = 1.0;
constraints.WeightY = 0.0; // No vertical extra space

// Control takes both horizontal and vertical extra space
constraints.WeightX = 1.0;
constraints.WeightY = 1.0;

// Control takes 2x horizontal space compared to others with WeightX=1.0
constraints.WeightX = 2.0;
constraints.WeightY = 0.0;
```

## Cell Spanning (CellSpanX, CellSpanY)

### Purpose

Allow a control to span multiple columns and/or rows.

### Properties

| Property | Description | Default |
|----------|-------------|---------|
| **CellSpanX** | Number of columns to span | null (1) |
| **CellSpanY** | Number of rows to span | null (1) |

### Usage

```csharp
// Span 2 columns, 1 row
GridBagConstraints constraints = new GridBagConstraints(
    0, 0,  // Position
    2, 1,  // Span: 2 columns, 1 row
    1.0, 0.0, AnchorTypes.Center, FillType.Horizontal,
    new Insets(0, 0, 0, 0), 0, 0, false
);

// Span 1 column, 3 rows
constraints.CellSpanX = 1;
constraints.CellSpanY = 3;
```

### Example: Submit Button Spanning 2 Columns

```csharp
// Layout: Label | TextBox
//         Label | TextBox
//         [   Submit Button   ]  ← Spans 2 columns

// Submit button at row 2, spans 2 columns
GridBagConstraints submitConstraints = new GridBagConstraints(
    0, 2,  // Position: column 0, row 2
    2, 1,  // Span: 2 columns, 1 row
    1.0, 0.0, AnchorTypes.Center, FillType.None,
    new Insets(10, 0, 0, 0), 0, 0, false
);
```

## Anchor Property

### Purpose

Specify where the control is positioned within its allocated cell area (if the control is smaller than the cell).

### Anchor Types

| Anchor | Description | Visual Position |
|--------|-------------|-----------------|
| **Center** | Center of cell (default) | Middle |
| **North** | Top center | ↑ |
| **South** | Bottom center | ↓ |
| **East** | Right center | → |
| **West** | Left center | ← |
| **NorthEast** | Top-right corner | ↗ |
| **NorthWest** | Top-left corner | ↖ |
| **SouthEast** | Bottom-right corner | ↘ |
| **SouthWest** | Bottom-left corner | ↙ |

### Usage

```csharp
// Anchor label to right side (common for form labels)
constraints.Anchor = AnchorTypes.East;

// Anchor button to bottom-right corner
constraints.Anchor = AnchorTypes.SouthEast;

// Center control (default)
constraints.Anchor = AnchorTypes.Center;
```

### Common Pattern: Form Labels

```csharp
// Labels right-aligned (anchor East)
labelConstraints.Anchor = AnchorTypes.East;

// Fields fill horizontal space
fieldConstraints.Fill = FillType.Horizontal;
fieldConstraints.Anchor = AnchorTypes.West; // Or use Fill instead
```

## Fill Property

### Purpose

Specify whether the control should resize to fill its allocated cell area.

### Fill Types

| Fill Type | Description | Behavior |
|-----------|-------------|----------|
| **None** | Don't resize control | Control stays at preferred size, positioned by Anchor |
| **Horizontal** | Fill cell width | Control width matches cell width |
| **Vertical** | Fill cell height | Control height matches cell height |
| **Both** | Fill entire cell | Control fills entire cell area |

### Usage

```csharp
// TextBox fills horizontal space
constraints.Fill = FillType.Horizontal;

// Button fills entire cell
constraints.Fill = FillType.Both;

// Label doesn't resize (use anchor for positioning)
constraints.Fill = FillType.None;
constraints.Anchor = AnchorTypes.East; // Right-align
```

### Common Patterns

**TextBox (single-line):**
```csharp
constraints.Fill = FillType.Horizontal; // Fill width, fixed height
```

**TextBox (multi-line):**
```csharp
constraints.Fill = FillType.Both; // Fill both dimensions
```

**Button (standard):**
```csharp
constraints.Fill = FillType.None; // Keep button size, use Anchor
constraints.Anchor = AnchorTypes.East; // Right-align
```

**Button (full-width):**
```csharp
constraints.Fill = FillType.Horizontal; // Fill width
```

## Internal Padding (IPadX, IPadY)

### Purpose

Add pixels to the control's preferred size when calculating layout (increases control size).

### Properties

| Property | Description | Default |
|----------|-------------|---------|
| **IPadX** | Pixels added to preferred width | null (0) |
| **IPadY** | Pixels added to preferred height | null (0) |

### Usage

```csharp
// Add 10 pixels to preferred width, 5 to preferred height
constraints.IPadX = 10;
constraints.IPadY = 5;
```

**Effect:** If control's preferred size is 100x30, with IPadX=10 and IPadY=5, the layout manager treats it as 110x35.

### When to Use

- Make controls slightly larger than default
- Add breathing room to buttons
- Increase clickable area

**Note:** Insets are usually preferred for adding space (they add padding around control, not to control itself).

## Insets (Cell Padding)

### Purpose

Add padding (space) around the control within its cell. Unlike IPadding, this doesn't increase the control's size - it creates space between the control and cell edges.

### Insets Class

```csharp
Insets(int top, int left, int bottom, int right)
```

### Usage

```csharp
// 5 pixels padding on all sides
constraints.Insets = new Insets(5, 5, 5, 5);

// 10 pixels top, 5 pixels left/right, 0 bottom
constraints.Insets = new Insets(10, 5, 0, 5);

// No padding
constraints.Insets = new Insets(0, 0, 0, 0);
```

### Common Patterns

**Uniform padding (all controls):**
```csharp
constraints.Insets = new Insets(5, 5, 5, 5);
```

**Top spacing (separate sections):**
```csharp
constraints.Insets = new Insets(20, 5, 5, 5); // Extra top spacing
```

**No padding (adjacent controls):**
```csharp
constraints.Insets = new Insets(0, 0, 0, 0);
```

## Adding Controls via Designer

### Step-by-Step Designer Usage

1. **Add GridBagLayout:**
   - Drag `GridBagLayout` from Toolbox to form
   - Appears in component tray
   - Popup asks if form should be container → Click **Yes**

2. **Set ContainerControl (if not using Form):**
   - Select GridBagLayout in component tray
   - Set `ContainerControl` property to Panel

3. **Add Child Controls:**
   - Drag controls (labels, textboxes, buttons) onto container
   - Controls are added to grid

4. **Set Constraints (Properties Window):**
   - Select each child control
   - In Properties, find extended properties:
     - GridPosX, GridPosY (position)
     - CellSpanX, CellSpanY (spanning)
     - WeightX, WeightY (resize behavior)
     - Anchor (positioning within cell)
     - Fill (resize mode)
     - Insets (padding)
     - IPadX, IPadY (internal padding)

5. **Rearrange at Design Time:**
   - Drag and drop controls in designer to reposition
   - GridBag updates constraints automatically

### Designer Tips

- Use Properties window for precise constraint control
- Test resize behavior by resizing form in designer
- Set WeightX/WeightY to see space distribution

## Adding Controls via Code

### Complete Code Example (Data Entry Form)

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class DataEntryForm : Form
{
    private Panel panel1;
    private GridBagLayout gridBagLayout1;

    public DataEntryForm()
    {
        this.Size = new Size(500, 300);
        this.Text = "Data Entry Form";

        // Create container panel
        panel1 = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = Color.WhiteSmoke
        };
        this.Controls.Add(panel1);

        // Create GridBagLayout
        gridBagLayout1 = new GridBagLayout();
        gridBagLayout1.ContainerControl = panel1;

        // Create form controls
        Label nameLabel = new Label { Text = "Name:", AutoSize = true };
        TextBox nameTextBox = new TextBox { Width = 200 };

        Label emailLabel = new Label { Text = "Email:", AutoSize = true };
        TextBox emailTextBox = new TextBox { Width = 200 };

        Label commentsLabel = new Label { Text = "Comments:", AutoSize = true };
        TextBox commentsTextBox = new TextBox { Multiline = true, Width = 200, Height = 60 };

        Button submitButton = new Button { Text = "Submit", Width = 100 };
        Button cancelButton = new Button { Text = "Cancel", Width = 100 };

        // Add controls to container
        panel1.Controls.AddRange(new Control[] {
            nameLabel, nameTextBox,
            emailLabel, emailTextBox,
            commentsLabel, commentsTextBox,
            submitButton, cancelButton
        });

        // Set constraints for Name label (column 0, row 0)
        gridBagLayout1.SetConstraints(nameLabel, new GridBagConstraints(
            0, 0, 1, 1,         // Position and span
            0.0, 0.0,           // No extra space
            AnchorTypes.East,   // Right-align
            FillType.None,
            new Insets(10, 10, 5, 10),
            0, 0, false
        ));

        // Set constraints for Name textbox (column 1, row 0)
        gridBagLayout1.SetConstraints(nameTextBox, new GridBagConstraints(
            1, 0, 1, 1,         // Position and span
            1.0, 0.0,           // Take horizontal extra space
            AnchorTypes.West,
            FillType.Horizontal, // Fill width
            new Insets(10, 0, 5, 10),
            0, 0, false
        ));

        // Set constraints for Email label (column 0, row 1)
        gridBagLayout1.SetConstraints(emailLabel, new GridBagConstraints(
            0, 1, 1, 1,
            0.0, 0.0,
            AnchorTypes.East,
            FillType.None,
            new Insets(5, 10, 5, 10),
            0, 0, false
        ));

        // Set constraints for Email textbox (column 1, row 1)
        gridBagLayout1.SetConstraints(emailTextBox, new GridBagConstraints(
            1, 1, 1, 1,
            1.0, 0.0,
            AnchorTypes.West,
            FillType.Horizontal,
            new Insets(5, 0, 5, 10),
            0, 0, false
        ));

        // Set constraints for Comments label (column 0, row 2)
        gridBagLayout1.SetConstraints(commentsLabel, new GridBagConstraints(
            0, 2, 1, 1,
            0.0, 0.0,
            AnchorTypes.NorthEast, // Top-right align
            FillType.None,
            new Insets(5, 10, 5, 10),
            0, 0, false
        ));

        // Set constraints for Comments textbox (column 1, row 2)
        gridBagLayout1.SetConstraints(commentsTextBox, new GridBagConstraints(
            1, 2, 1, 1,
            1.0, 1.0,           // Take both horizontal and vertical space
            AnchorTypes.West,
            FillType.Both,      // Fill both dimensions
            new Insets(5, 0, 5, 10),
            0, 0, false
        ));

        // Set constraints for Submit button (column 0, row 3)
        gridBagLayout1.SetConstraints(submitButton, new GridBagConstraints(
            0, 3, 1, 1,
            0.0, 0.0,
            AnchorTypes.East,   // Right-align
            FillType.None,
            new Insets(15, 10, 10, 5),
            0, 0, false
        ));

        // Set constraints for Cancel button (column 1, row 3)
        gridBagLayout1.SetConstraints(cancelButton, new GridBagConstraints(
            1, 3, 1, 1,
            0.0, 0.0,
            AnchorTypes.West,   // Left-align
            FillType.None,
            new Insets(15, 5, 10, 10),
            0, 0, false
        ));

        // Event handlers
        submitButton.Click += (s, e) => MessageBox.Show("Form submitted!");
        cancelButton.Click += (s, e) => this.Close();
    }

    [STAThread]
    static void Main()
    {
        Application.Run(new DataEntryForm());
    }
}
```

```vbnet
' VB.NET version follows same pattern
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class DataEntryForm
    Inherits Form

    Private panel1 As Panel
    Private gridBagLayout1 As GridBagLayout

    Public Sub New()
        ' Implementation similar to C# version
        ' Create GridBagLayout, add controls, set constraints
    End Sub

    <STAThread>
    Shared Sub Main()
        Application.Run(New DataEntryForm())
    End Sub
End Class
```

## Complete Examples

### Example 1: Login Form

```csharp
// Layout: Username label | Username textbox
//         Password label | Password textbox
//         [      Login Button       ]  ← Spans 2 columns

GridBagLayout gridBagLayout1 = new GridBagLayout();
gridBagLayout1.ContainerControl = panel1;

Label usernameLabel = new Label { Text = "Username:", AutoSize = true };
TextBox usernameTextBox = new TextBox();

Label passwordLabel = new Label { Text = "Password:", AutoSize = true };
TextBox passwordTextBox = new TextBox { PasswordChar = '*' };

Button loginButton = new Button { Text = "Login", Width = 100 };

panel1.Controls.AddRange(new Control[] {
    usernameLabel, usernameTextBox,
    passwordLabel, passwordTextBox,
    loginButton
});

// Username label: column 0, row 0
gridBagLayout1.SetConstraints(usernameLabel, new GridBagConstraints(
    0, 0, 1, 1, 0.0, 0.0, AnchorTypes.East, FillType.None,
    new Insets(10, 10, 5, 10), 0, 0, false
));

// Username textbox: column 1, row 0
gridBagLayout1.SetConstraints(usernameTextBox, new GridBagConstraints(
    1, 0, 1, 1, 1.0, 0.0, AnchorTypes.West, FillType.Horizontal,
    new Insets(10, 0, 5, 10), 0, 0, false
));

// Password label: column 0, row 1
gridBagLayout1.SetConstraints(passwordLabel, new GridBagConstraints(
    0, 1, 1, 1, 0.0, 0.0, AnchorTypes.East, FillType.None,
    new Insets(5, 10, 5, 10), 0, 0, false
));

// Password textbox: column 1, row 1
gridBagLayout1.SetConstraints(passwordTextBox, new GridBagConstraints(
    1, 1, 1, 1, 1.0, 0.0, AnchorTypes.West, FillType.Horizontal,
    new Insets(5, 0, 5, 10), 0, 0, false
));

// Login button: column 0-1, row 2 (spans 2 columns)
gridBagLayout1.SetConstraints(loginButton, new GridBagConstraints(
    0, 2, 2, 1, 0.0, 0.0, AnchorTypes.Center, FillType.None,
    new Insets(15, 10, 10, 10), 0, 0, false
));
```

### Example 2: Calculator Layout

```csharp
// Layout: [    Display    ]  ← Spans 4 columns
//         [7] [8] [9] [/]
//         [4] [5] [6] [*]
//         [1] [2] [3] [-]
//         [0]  spans 2   [+]

GridBagLayout gridBagLayout1 = new GridBagLayout();
gridBagLayout1.ContainerControl = panel1;

TextBox display = new TextBox { Text = "0", TextAlign = HorizontalAlignment.Right, ReadOnly = true };

// Create number buttons
Button[] numberButtons = new Button[10];
for (int i = 0; i <= 9; i++)
{
    numberButtons[i] = new Button { Text = i.ToString(), Width = 50, Height = 50 };
}

// Create operator buttons
Button btnAdd = new Button { Text = "+", Width = 50, Height = 50 };
Button btnSubtract = new Button { Text = "-", Width = 50, Height = 50 };
Button btnMultiply = new Button { Text = "*", Width = 50, Height = 50 };
Button btnDivide = new Button { Text = "/", Width = 50, Height = 50 };

// Add all controls
panel1.Controls.Add(display);
panel1.Controls.AddRange(numberButtons);
panel1.Controls.AddRange(new Control[] { btnAdd, btnSubtract, btnMultiply, btnDivide });

// Display: row 0, spans 4 columns
gridBagLayout1.SetConstraints(display, new GridBagConstraints(
    0, 0, 4, 1, 1.0, 0.0, AnchorTypes.Center, FillType.Horizontal,
    new Insets(5, 5, 5, 5), 0, 0, false
));

// Button 7: column 0, row 1
gridBagLayout1.SetConstraints(numberButtons[7], new GridBagConstraints(
    0, 1, 1, 1, 0.0, 0.0, AnchorTypes.Center, FillType.Both,
    new Insets(2, 2, 2, 2), 0, 0, false
));

// Button 8: column 1, row 1
gridBagLayout1.SetConstraints(numberButtons[8], new GridBagConstraints(
    1, 1, 1, 1, 0.0, 0.0, AnchorTypes.Center, FillType.Both,
    new Insets(2, 2, 2, 2), 0, 0, false
));

// Button 9: column 2, row 1
gridBagLayout1.SetConstraints(numberButtons[9], new GridBagConstraints(
    2, 1, 1, 1, 0.0, 0.0, AnchorTypes.Center, FillType.Both,
    new Insets(2, 2, 2, 2), 0, 0, false
));

// Divide button: column 3, row 1
gridBagLayout1.SetConstraints(btnDivide, new GridBagConstraints(
    3, 1, 1, 1, 0.0, 0.0, AnchorTypes.Center, FillType.Both,
    new Insets(2, 2, 2, 2), 0, 0, false
));

// Continue for rows 2-4...
// Button 0 spans 2 columns at row 4
gridBagLayout1.SetConstraints(numberButtons[0], new GridBagConstraints(
    0, 4, 2, 1, 0.0, 0.0, AnchorTypes.Center, FillType.Both,
    new Insets(2, 2, 2, 2), 0, 0, false
));
```

## Advanced Spanning Scenarios

### Multi-Column Header

```csharp
// Header spans entire width (4 columns)
Label header = new Label { Text = "Registration Form", Font = new Font("Arial", 16, FontStyle.Bold) };

gridBagLayout1.SetConstraints(header, new GridBagConstraints(
    0, 0, 4, 1,         // Span 4 columns
    1.0, 0.0,
    AnchorTypes.Center,
    FillType.Horizontal,
    new Insets(10, 10, 20, 10),
    0, 0, false
));
```

### Multi-Row Sidebar

```csharp
// Sidebar spans multiple rows (3 rows)
Panel sidebar = new Panel { BackColor = Color.LightGray, Width = 150 };

gridBagLayout1.SetConstraints(sidebar, new GridBagConstraints(
    0, 0, 1, 3,         // Span 3 rows
    0.0, 1.0,           // Take vertical space
    AnchorTypes.Center,
    FillType.Both,
    new Insets(5, 5, 5, 5),
    0, 0, false
));
```

### Large Multi-Line TextBox

```csharp
// TextBox spans 2 columns and 3 rows
TextBox largeTextBox = new TextBox { Multiline = true, ScrollBars = ScrollBars.Vertical };

gridBagLayout1.SetConstraints(largeTextBox, new GridBagConstraints(
    1, 1, 2, 3,         // Span 2 columns, 3 rows
    1.0, 1.0,           // Take both horizontal and vertical space
    AnchorTypes.Center,
    FillType.Both,
    new Insets(5, 5, 5, 5),
    0, 0, false
));
```

## Common Patterns

### Pattern 1: Form Layout (Labels + Fields)

**Structure:** Labels in column 0 (right-aligned), fields in column 1 (fill horizontal)

```csharp
// Label constraints (reusable for all labels)
GridBagConstraints labelConstraints = new GridBagConstraints(
    0, 0, 1, 1, 0.0, 0.0, AnchorTypes.East, FillType.None,
    new Insets(5, 10, 5, 10), 0, 0, false
);

// Field constraints (reusable for all fields)
GridBagConstraints fieldConstraints = new GridBagConstraints(
    1, 0, 1, 1, 1.0, 0.0, AnchorTypes.West, FillType.Horizontal,
    new Insets(5, 0, 5, 10), 0, 0, false
);

// Apply to each label/field pair, adjusting GridPosY
for (int row = 0; row < 5; row++)
{
    labelConstraints.GridPosY = row;
    fieldConstraints.GridPosY = row;
    
    gridBagLayout1.SetConstraints(labels[row], labelConstraints);
    gridBagLayout1.SetConstraints(fields[row], fieldConstraints);
}
```

### Pattern 2: Button Row at Bottom

**Structure:** Multiple buttons in a row, right-aligned

```csharp
// OK button: column 0, last row
gridBagLayout1.SetConstraints(btnOK, new GridBagConstraints(
    0, lastRow, 1, 1, 0.0, 0.0, AnchorTypes.East, FillType.None,
    new Insets(15, 10, 10, 5), 0, 0, false
));

// Cancel button: column 1, last row
gridBagLayout1.SetConstraints(btnCancel, new GridBagConstraints(
    1, lastRow, 1, 1, 0.0, 0.0, AnchorTypes.West, FillType.None,
    new Insets(15, 5, 10, 10), 0, 0, false
));
```

### Pattern 3: Full-Width Button at Bottom

**Structure:** Button spans all columns

```csharp
// Submit button spans 2 columns
gridBagLayout1.SetConstraints(btnSubmit, new GridBagConstraints(
    0, lastRow, 2, 1,   // Span 2 columns
    0.0, 0.0,
    AnchorTypes.Center,
    FillType.Horizontal, // Or FillType.None for fixed width
    new Insets(20, 10, 10, 10),
    0, 0, false
));
```

## Best Practices

1. **Plan Grid Structure on Paper First**
   - Sketch the layout with row/column numbers
   - Identify spanning requirements
   - Mark which columns/rows should take extra space

2. **Reuse GridBagConstraints Objects**
   - Create template constraints for similar controls
   - Adjust only GridPosX/GridPosY for each control
   - Reduces code duplication

```csharp
GridBagConstraints labelConstraints = new GridBagConstraints(
    0, 0, 1, 1, 0.0, 0.0, AnchorTypes.East, FillType.None,
    new Insets(5, 10, 5, 10), 0, 0, false
);

// Reuse for all labels, just change row
for (int i = 0; i < labels.Length; i++)
{
    labelConstraints.GridPosY = i;
    gridBagLayout1.SetConstraints(labels[i], labelConstraints);
}
```

3. **Use WeightX/WeightY for Proper Resize Behavior**
   - TextBoxes: `WeightX = 1.0` (take horizontal space)
   - Multi-line TextBox: `WeightX = 1.0, WeightY = 1.0` (take both)
   - Labels: `WeightX = 0.0, WeightY = 0.0` (fixed size)
   - Buttons: Usually `WeightX = 0.0, WeightY = 0.0`

4. **Set Fill.Horizontal for Text Boxes**
   - TextBoxes should fill available width
   - Use `FillType.Horizontal` for single-line
   - Use `FillType.Both` for multi-line

5. **Anchor Labels to East (Right-Align)**
   - Form labels look better right-aligned next to fields
   - Use `AnchorTypes.East` for labels

6. **Use Insets for Consistent Spacing**
   - Apply uniform insets to all controls: `new Insets(5, 5, 5, 5)`
   - Increase top inset for section separators: `new Insets(20, 5, 5, 5)`

7. **Test Resize Behavior Thoroughly**
   - Resize form to minimum and maximum sizes
   - Ensure controls resize/reposition correctly
   - Verify WeightX/WeightY distribution

8. **Use AutoSize for Labels**
   - Labels with `AutoSize = true` adapt to text length
   - Grid adjusts column width accordingly

9. **Don't Overlap Controls Unintentionally**
   - Ensure unique GridPosX/GridPosY combinations
   - Overlapping is usually a mistake

10. **Document Complex Layouts**
    - Add comments explaining grid structure
    - Note which rows/columns have weights
    - Explain spanning decisions

## Troubleshooting

### Controls Not Appearing

**Problem:** Controls added but not visible

**Solutions:**
- Verify `ContainerControl` is set
- Check GridPosX/GridPosY are non-negative
- Ensure controls were added to container: `panel.Controls.Add(control)`
- Call `gridBagLayout1.LayoutContainer()` to force layout
- Check if control is outside visible grid (too large GridPosX/GridPosY)

### Unexpected Sizing

**Problem:** Controls are too small or too large

**Solutions:**
- Review `Fill` property (None, Horizontal, Vertical, Both)
- Check `WeightX/WeightY` values
- Verify `Insets` aren't too large
- Set `PreferredSize` or `MinimumSize` if needed
- Check `IPadX/IPadY` values

### Poor Resize Behavior

**Problem:** Layout doesn't adapt well when form resizes

**Solutions:**
- Set `WeightX = 1.0` for columns that should take extra horizontal space
- Set `WeightY = 1.0` for rows that should take extra vertical space
- Check that at least one column has WeightX > 0 and one row has WeightY > 0
- Use `Fill.Horizontal` or `Fill.Both` for controls that should resize

### Overlapping Controls

**Problem:** Multiple controls visible in same cell

**Solutions:**
- Verify GridPosX/GridPosY values are unique (unless overlap is intentional)
- Check spanning (CellSpanX/CellSpanY) doesn't cause unintended overlap
- Review grid structure on paper

### Spacing Issues

**Problem:** Controls too close together or too far apart

**Solutions:**
- Adjust `Insets` for uniform spacing: `new Insets(5, 5, 5, 5)`
- Increase insets for more space: `new Insets(10, 10, 10, 10)`
- Use `IPadX/IPadY` if control itself needs to be larger
- Add empty rows/columns with minimal controls for spacing

### Anchor Not Working

**Problem:** Anchor property doesn't seem to affect control position

**Solutions:**
- Ensure `Fill = FillType.None` (Fill overrides Anchor)
- Check that cell is larger than control (Anchor positions control within cell)
- Verify Anchor value is correct (`AnchorTypes.East`, not just `East`)

### Spanning Not Working

**Problem:** Control doesn't span multiple cells

**Solutions:**
- Check `CellSpanX` and `CellSpanY` values
- Ensure spanned cells exist in grid (GridPosX + CellSpanX ≤ grid width)
- Verify other controls aren't blocking spanning
- Set `Fill = FillType.Horizontal` or `Fill.Both` to see spanning effect

---

## See Also

- [GridBagLayout API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.GridBagLayout.html)
- [GridBagConstraints API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.GridBagConstraints.html)
- [LayoutManagers Overview](https://help.syncfusion.com/windowsforms/layoutmanagers/overview)
