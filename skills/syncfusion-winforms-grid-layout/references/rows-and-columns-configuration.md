# Rows and Columns Configuration

## Table of Contents
- [Overview](#overview)
- [Rows Property](#rows-property)
- [Columns Property](#columns-property)
- [Property Precedence](#property-precedence)
- [Auto-Sizing Behavior](#auto-sizing-behavior)
- [Configuration Examples](#configuration-examples)

## Overview

GridLayout divides the available space into rows and columns based on the number of child controls. You can explicitly configure the number of rows and columns using the `Rows` and `Columns` properties.

**Key Properties:**

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| **Rows** | int | null | Number of rows in the grid |
| **Columns** | int | null | Number of columns in the grid |

## Rows Property

The `Rows` property specifies the number of rows that the GridLayout will create.

**Behavior:**
- When `Rows` is set to a positive value, it dictates the grid structure
- The `Rows` property takes precedence over `Columns` when both are set
- When `Rows` is set, the number of columns is automatically calculated based on the number of child controls
- If you have 6 controls and set `Rows = 2`, columns will be calculated as 3 (2 rows × 3 columns = 6 controls)

**Setting Rows in Code:**

C#:
```csharp
this.gridLayout1.Rows = 2;
```

VB.NET:
```vb
Me.gridLayout1.Rows = 2
```

**When to Use:**
- You know how many rows you want (e.g., always 2 rows)
- You want columns to adjust automatically based on content
- Creating fixed row structures like header-content-footer layouts

## Columns Property

The `Columns` property specifies the number of columns that the GridLayout will create.

**Behavior:**
- When `Rows` is null or negative, `Columns` dictates the grid structure
- The number of rows is automatically calculated based on child controls
- If you have 6 controls and set `Columns = 3`, rows will be calculated as 2 (2 rows × 3 columns = 6 controls)
- If `Rows` is set to a positive value, `Columns` is overridden

**Setting Columns in Code:**

C#:
```csharp
this.gridLayout1.Columns = 3;
```

VB.NET:
```vb
Me.gridLayout1.Columns = 3
```

**When to Use:**
- You know how many columns you want (e.g., always 3 columns)
- You want rows to adjust automatically based on content
- Creating responsive layouts that reflow based on content

## Property Precedence

**Important:** The `Rows` property takes precedence over `Columns`.

**Scenario 1: Both Rows and Columns Set**
```csharp
gridLayout1.Rows = 2;
gridLayout1.Columns = 3;
// Result: 2 rows, columns auto-calculated (Rows takes precedence)
```

**Scenario 2: Only Columns Set**
```csharp
gridLayout1.Rows = null;  // or not set
gridLayout1.Columns = 3;
// Result: 3 columns, rows auto-calculated
```

**Scenario 3: Only Rows Set**
```csharp
gridLayout1.Rows = 2;
gridLayout1.Columns = null;  // or not set
// Result: 2 rows, columns auto-calculated
```

**Scenario 4: Neither Set**
```csharp
// gridLayout1.Rows = null;
// gridLayout1.Columns = null;
// Result: Auto-arrangement based on child control count
```

## Auto-Sizing Behavior

When you don't explicitly set both `Rows` and `Columns`, GridLayout automatically calculates the missing dimension:

**Formula:**
```
If Rows is set (and positive):
    Columns = Ceiling(ChildControlCount / Rows)

If Columns is set (and Rows is null or negative):
    Rows = Ceiling(ChildControlCount / Columns)

If both are null:
    Single row is created and adjusted based on container width
```

**Example: Auto-Calculating Columns**

With 10 controls and `Rows = 3`:
```
Columns = Ceiling(10 / 3) = 4
Result: 3 rows × 4 columns grid (12 cells, 2 empty)
```

**Example: Auto-Calculating Rows**

With 10 controls and `Columns = 3`:
```
Rows = Ceiling(10 / 3) = 4
Result: 4 rows × 3 columns grid (12 cells, 2 empty)
```

## Configuration Examples

### Example 1: Fixed 2-Row Layout

Creating a form with two rows of controls:

C#:
```csharp
GridLayout gridLayout1 = new GridLayout();
gridLayout1.ContainerControl = this;

// Configure for 2 rows
gridLayout1.Rows = 2;
gridLayout1.HGap = 5;
gridLayout1.VGap = 5;

// Add 4 controls (will arrange in 2×2 grid)
for (int i = 0; i < 4; i++)
{
    ButtonAdv btn = new ButtonAdv() { Text = $"Button {i + 1}" };
    this.Controls.Add(btn);
    gridLayout1.SetParticipateInLayout(btn, true);
}
```

**Result:** 2 rows × 2 columns grid

### Example 2: Dynamic Column Layout

Creating a responsive 3-column layout:

C#:
```csharp
GridLayout gridLayout1 = new GridLayout();
gridLayout1.ContainerControl = this;

// Configure for 3 columns
gridLayout1.Columns = 3;
gridLayout1.HGap = 10;
gridLayout1.VGap = 10;

// Add controls (will arrange in 3 columns, rows auto-calculated)
for (int i = 0; i < 9; i++)
{
    ButtonAdv btn = new ButtonAdv() { Text = $"Button {i + 1}" };
    this.Controls.Add(btn);
    gridLayout1.SetParticipateInLayout(btn, true);
}
```

**Result:** Rows auto-calculated as 3 (9 controls ÷ 3 columns = 3 rows)

### Example 3: Override Columns with Rows

C#:
```csharp
GridLayout gridLayout1 = new GridLayout();
gridLayout1.ContainerControl = this;

// Initially set columns
gridLayout1.Columns = 4;

// Later, set rows (this will override columns setting)
gridLayout1.Rows = 3;

// Now the grid will be 3 rows with auto-calculated columns
```

### Example 4: Changing Grid Structure at Runtime

C#:
```csharp
// Initial 2-row layout
gridLayout1.Rows = 2;

// User clicks button to change layout
private void buttonChangeLayout_Click(object sender, EventArgs e)
{
    gridLayout1.Rows = 3;  // Change to 3 rows
    // Grid automatically recalculates with new row count
}
```

**Key Point:** Changing `Rows` or `Columns` at runtime automatically triggers a layout recalculation for all participating controls.
