# Appearance Customization

## Table of Contents
- [Border Styles](#border-styles)
- [3D Border Effects](#3d-border-effects)
- [Border Color and Sides](#border-color-and-sides)
- [Color Coding by Value](#color-coding-by-value)
- [Visual Style Application](#visual-style-application)

## Border Styles

### BorderStyle Property

Sets the basic border appearance:

```csharp
// No border (flat appearance)
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.None;

// Single-line border (default)
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
// Display: Thin black line around control

// 3D inset border (appears recessed)
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
```

### BorderStyle Examples

```csharp
// Minimal design (no border)
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.None;
currencyTextBox1.BackColor = System.Drawing.Color.LightGray;
currencyTextBox1.Padding = new System.Windows.Forms.Padding(5);

// Classic design (single border)
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
currencyTextBox1.BorderColor = System.Drawing.Color.Gray;

// Modern design (3D effect)
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
```

## 3D Border Effects

### Border3DStyle Property

Applies 3D visual effects when BorderStyle is Fixed3D:

```csharp
// Raised appearance (button-like)
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
currencyTextBox1.Border3DStyle = System.Windows.Forms.Border3DStyle.Raised;
// Effect: Looks elevated or embossed

// Sunken appearance (input field-like)
currencyTextBox1.Border3DStyle = System.Windows.Forms.Border3DStyle.Sunken;
// Effect: Looks recessed or depressed

// Flat appearance (no 3D effect)
currencyTextBox1.Border3DStyle = System.Windows.Forms.Border3DStyle.Flat;

// Etched appearance (engraved)
currencyTextBox1.Border3DStyle = System.Windows.Forms.Border3DStyle.Etched;

// Bump appearance (beveled edge)
currencyTextBox1.Border3DStyle = System.Windows.Forms.Border3DStyle.Bump;
```

### 3D Style Examples

```csharp
// Classic form field (sunken)
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
currencyTextBox1.Border3DStyle = System.Windows.Forms.Border3DStyle.Sunken;

// Dialog box style (raised)
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
currencyTextBox1.Border3DStyle = System.Windows.Forms.Border3DStyle.Raised;

// Modern flat design
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
currencyTextBox1.BorderColor = System.Drawing.Color.LightBlue;
```

## Border Color and Sides

### BorderColor Property

Set the color of the border line:

```csharp
// Black border (default)
currencyTextBox1.BorderColor = System.Drawing.Color.Black;

// Blue border
currencyTextBox1.BorderColor = System.Drawing.Color.Blue;

// Red border (error state)
currencyTextBox1.BorderColor = System.Drawing.Color.Red;

// Green border (success state)
currencyTextBox1.BorderColor = System.Drawing.Color.Green;

// Custom color
currencyTextBox1.BorderColor = System.Drawing.Color.FromArgb(100, 150, 200);

// Transparent/lighter border
currencyTextBox1.BorderColor = System.Drawing.Color.LightGray;
```

### BorderSides Property

Control which sides have borders:

```csharp
// All sides (default)
currencyTextBox1.BorderSides = System.Windows.Forms.Border3DSide.All;

// Top and bottom only
currencyTextBox1.BorderSides = System.Windows.Forms.Border3DSide.Top | 
                               System.Windows.Forms.Border3DSide.Bottom;

// Left and right only
currencyTextBox1.BorderSides = System.Windows.Forms.Border3DSide.Left | 
                               System.Windows.Forms.Border3DSide.Right;

// Top only
currencyTextBox1.BorderSides = System.Windows.Forms.Border3DSide.Top;

// Bottom only (underline effect)
currencyTextBox1.BorderSides = System.Windows.Forms.Border3DSide.Bottom;

// No border
currencyTextBox1.BorderSides = System.Windows.Forms.Border3DSide.None;
```

### Partial Border Examples

```csharp
// Underline style (bottom border only)
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
currencyTextBox1.BorderSides = System.Windows.Forms.Border3DSide.Bottom;
currencyTextBox1.BorderColor = System.Drawing.Color.DarkBlue;

// Left accent line
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
currencyTextBox1.BorderSides = System.Windows.Forms.Border3DSide.Left;
currencyTextBox1.BorderColor = System.Drawing.Color.Teal;

// Top and bottom (sandwich style)
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
currencyTextBox1.BorderSides = System.Windows.Forms.Border3DSide.Top | 
                               System.Windows.Forms.Border3DSide.Bottom;
currencyTextBox1.BorderColor = System.Drawing.Color.Silver;
```

## Color Coding by Value

### PositiveColor Property

Color for positive (greater than zero) values:

```csharp
// Default: black text
currencyTextBox1.PositiveColor = System.Drawing.Color.Black;

// Green for profit/gain
currencyTextBox1.PositiveColor = System.Drawing.Color.Green;
currencyTextBox1.DecimalValue = 1000m;
// Display: 1000.00 in green text

// Blue for income
currencyTextBox1.PositiveColor = System.Drawing.Color.Blue;

// Dark green for profit
currencyTextBox1.PositiveColor = System.Drawing.Color.DarkGreen;
```

### NegativeColor Property

Color for negative (less than zero) values:

```csharp
// Red for loss/debt
currencyTextBox1.NegativeColor = System.Drawing.Color.Red;
currencyTextBox1.DecimalValue = -500m;
// Display: -500.00 in red text

// Dark red
currencyTextBox1.NegativeColor = System.Drawing.Color.DarkRed;

// Maroon
currencyTextBox1.NegativeColor = System.Drawing.Color.Maroon;
```

### ZeroColor Property

Color for zero values:

```csharp
// Gray for neutral zero
currencyTextBox1.ZeroColor = System.Drawing.Color.Gray;
currencyTextBox1.DecimalValue = 0m;
// Display: 0.00 in gray text

// Dark gray
currencyTextBox1.ZeroColor = System.Drawing.Color.DarkGray;

// Same as positive (no distinction)
currencyTextBox1.ZeroColor = System.Drawing.Color.Black;
```

### Color Coding Example: P&L Statement

```csharp
// Profit and Loss Display
currencyTextBox1.PositiveColor = System.Drawing.Color.Green;     // Revenue
currencyTextBox1.NegativeColor = System.Drawing.Color.Red;       // Expenses
currencyTextBox1.ZeroColor = System.Drawing.Color.Black;         // Neutral

// Revenue row: $10,000 (green)
revenueBox.DecimalValue = 10000m;

// Expense row: -$3,000 (red)
expenseBox.DecimalValue = -3000m;

// Profit row: $7,000 (green)
profitBox.DecimalValue = 7000m;
```

### Color Coding Example: Account Balance

```csharp
// Account Balance Display
currencyTextBox1.PositiveColor = System.Drawing.Color.DarkGreen;  // Money available
currencyTextBox1.NegativeColor = System.Drawing.Color.DarkRed;    // Overdraft
currencyTextBox1.ZeroColor = System.Drawing.Color.Gray;           // No funds

// Healthy balance: $5,000 (dark green)
balanceBox.DecimalValue = 5000m;

// Overdraft: -$500 (dark red)
overdraftBox.DecimalValue = -500m;

// No balance: $0 (gray)
emptyBox.DecimalValue = 0m;
```

### Color Coding Example: Budget Variance

```csharp
// Budget vs Actual Variance
currencyTextBox1.PositiveColor = System.Drawing.Color.Green;      // Under budget
currencyTextBox1.NegativeColor = System.Drawing.Color.Red;        // Over budget
currencyTextBox1.ZeroColor = System.Drawing.Color.Black;          // On budget

// Under budget: $2,000 (green)
underBox.DecimalValue = 2000m;

// Over budget: -$1,500 (red)
overBox.DecimalValue = -1500m;

// On budget: $0 (black)
onTargetBox.DecimalValue = 0m;
```

## ReadOnly Background Color

### ReadOnlyBackColor Property

Background color when control is read-only:

```csharp
// Set read-only mode
currencyTextBox1.ReadOnly = true;

// Light gray background for read-only
currencyTextBox1.ReadOnlyBackColor = System.Drawing.Color.LightGray;

// Very light gray (subtle)
currencyTextBox1.ReadOnlyBackColor = System.Drawing.Color.WhiteSmoke;

// Beige/tan (warm disabled look)
currencyTextBox1.ReadOnlyBackColor = System.Drawing.Color.Wheat;

// Light blue (locked appearance)
currencyTextBox1.ReadOnlyBackColor = System.Drawing.Color.LightBlue;
```

### Display-Only Field Example

```csharp
// Total field (display only)
currencyTextBox1.ReadOnly = true;
currencyTextBox1.ReadOnlyBackColor = System.Drawing.Color.LightGray;
currencyTextBox1.DecimalValue = 15000m;
// User sees value but cannot edit
// Background is light gray to indicate read-only status
```

## Complete Appearance Customization Example

### Example 1: Financial Form (Green/Red Coding)

```csharp
// Income field (positive numbers)
incomeBox.CurrencySymbol = "$";
incomeBox.CurrencyDecimalDigits = 2;
incomeBox.PositiveColor = System.Drawing.Color.DarkGreen;
incomeBox.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
incomeBox.BorderColor = System.Drawing.Color.DarkGreen;

// Expense field (negative numbers)
expenseBox.CurrencySymbol = "$";
expenseBox.CurrencyDecimalDigits = 2;
expenseBox.NegativeColor = System.Drawing.Color.DarkRed;
expenseBox.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
expenseBox.BorderColor = System.Drawing.Color.DarkRed;

// Net result field
netBox.CurrencySymbol = "$";
netBox.CurrencyDecimalDigits = 2;
netBox.ReadOnly = true;
netBox.ReadOnlyBackColor = System.Drawing.Color.LightGray;
netBox.PositiveColor = System.Drawing.Color.DarkGreen;
netBox.NegativeColor = System.Drawing.Color.DarkRed;
netBox.ZeroColor = System.Drawing.Color.Black;
```

### Example 2: Modern Flat Design

```csharp
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
currencyTextBox1.BorderColor = System.Drawing.Color.LightBlue;
currencyTextBox1.Font = new System.Drawing.Font("Segoe UI", 11);
currencyTextBox1.BackColor = System.Drawing.Color.White;
currencyTextBox1.PositiveColor = System.Drawing.Color.FromArgb(0, 102, 204);
currencyTextBox1.NegativeColor = System.Drawing.Color.FromArgb(204, 0, 0);
currencyTextBox1.ZeroColor = System.Drawing.Color.Gray;
currencyTextBox1.TextAlign = System.Windows.Forms.HorizontalAlignment.Right;
```

### Example 3: Minimalist Underline Style

```csharp
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
currencyTextBox1.BorderSides = System.Windows.Forms.Border3DSide.Bottom;
currencyTextBox1.BorderColor = System.Drawing.Color.DarkBlue;
currencyTextBox1.BackColor = System.Drawing.Color.Transparent;
currencyTextBox1.PositiveColor = System.Drawing.Color.Black;
currencyTextBox1.TextAlign = System.Windows.Forms.HorizontalAlignment.Right;
```

### Example 4: High Contrast (Accessibility)

```csharp
currencyTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
currencyTextBox1.BorderColor = System.Drawing.Color.Black;
currencyTextBox1.BackColor = System.Drawing.Color.White;
currencyTextBox1.Font = new System.Drawing.Font("Arial", 12, System.Drawing.FontStyle.Bold);
currencyTextBox1.PositiveColor = System.Drawing.Color.Black;
currencyTextBox1.NegativeColor = System.Drawing.Color.Red;
currencyTextBox1.ZeroColor = System.Drawing.Color.Black;
currencyTextBox1.ForeColor = System.Drawing.Color.Black;
```

## Visual Feedback States

### Error State (Invalid Input)

```csharp
private void ShowErrorState(CurrencyTextBox box)
{
    box.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
    box.BorderColor = System.Drawing.Color.Red;
    box.BackColor = System.Drawing.Color.MistyRose;
}

private void ClearErrorState(CurrencyTextBox box)
{
    box.BorderColor = System.Drawing.Color.Gray;
    box.BackColor = System.Drawing.Color.White;
}
```

### Focus State Feedback

```csharp
private void currencyTextBox1_Enter(object sender, EventArgs e)
{
    currencyTextBox1.BorderColor = System.Drawing.Color.Blue;
}

private void currencyTextBox1_Leave(object sender, EventArgs e)
{
    currencyTextBox1.BorderColor = System.Drawing.Color.Gray;
}
```

### Disabled State

```csharp
currencyTextBox1.Enabled = false;
currencyTextBox1.ReadOnlyBackColor = System.Drawing.Color.LightGray;
currencyTextBox1.ForeColor = System.Drawing.Color.Gray;
```
