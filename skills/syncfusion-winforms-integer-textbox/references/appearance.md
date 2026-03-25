# Visual Appearance & Styling

## Background Colors

Control the background appearance of the IntegerTextBox using background color properties.

### Standard Background Color

Set the normal background color using **BackColor**:

```csharp
this.integerTextBox1.BackColor = System.Drawing.Color.White;
```

**Common Background Colors:**

```csharp
// Light blue background
integerTextBox1.BackColor = System.Drawing.Color.LightBlue;

// Cream/beige
integerTextBox1.BackColor = System.Drawing.Color.PeachPuff;

// Light gray
integerTextBox1.BackColor = System.Drawing.Color.WhiteSmoke;

// Custom color
integerTextBox1.BackColor = System.Drawing.Color.FromArgb(220, 240, 255);
```

### Read-Only Background Color

When the control is read-only, set a distinct background using **ReadOnlyBackColor**:

```csharp
this.integerTextBox1.ReadOnly = true;
this.integerTextBox1.ReadOnlyBackColor = System.Drawing.Color.LavenderBlush;
```

**Important:** The ReadOnlyBackColor only applies when ReadOnly is set to **true**.

**Example: Disabled State Appearance**

```csharp
integerTextBox1.ReadOnly = true;
integerTextBox1.ReadOnlyBackColor = System.Drawing.Color.LightGray;
```

---

## Text Colors by Value Type

Display numbers in different colors based on whether they are positive, negative, or zero.

### Positive Values

Set the color for positive numbers using **PositiveColor**:

```csharp
this.integerTextBox1.PositiveColor = System.Drawing.Color.DarkGreen;
```

### Negative Values

Set the color for negative numbers using **NegativeColor**:

```csharp
this.integerTextBox1.NegativeColor = System.Drawing.Color.DarkRed;
```

### Zero Values

Set the color when the value is exactly zero using **ZeroColor**:

```csharp
this.integerTextBox1.ZeroColor = System.Drawing.Color.DarkGray;
```

### Complete Color Scheme Example

Set up a professional color coding system:

```csharp
// Color positive numbers green (profit/gain)
integerTextBox1.PositiveColor = System.Drawing.Color.DarkGreen;

// Color negative numbers red (loss/deficit)
integerTextBox1.NegativeColor = System.Drawing.Color.Crimson;

// Color zero values gray (neutral)
integerTextBox1.ZeroColor = System.Drawing.Color.Gray;
```

**Use Case: Financial Dashboard**

```csharp
// Display profit in green, loss in red
profitTextBox.PositiveColor = System.Drawing.Color.Green;
profitTextBox.NegativeColor = System.Drawing.Color.Red;
profitTextBox.PositiveColor = System.Drawing.Color.Black;  // Break-even
```

---

## Foreground Color

Set the default foreground (text) color using the inherited **ForeColor** property:

```csharp
// Set primary text color
integerTextBox1.ForeColor = System.Drawing.Color.Black;
```

---

## Border Styling

### Default Border

The IntegerTextBox displays a standard control border by default. To modify its appearance, use inherited properties:

```csharp
// Set border style (if available via BorderStyle property)
// Note: Border customization depends on the platform and form theme
integerTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
```

**BorderStyle Options:**

- **None** - No border
- **FixedSingle** - Single-line border
- **Fixed3D** - 3D beveled border

### Combined Styling

Create a visually distinct input field:

```csharp
integerTextBox1.BackColor = System.Drawing.Color.White;
integerTextBox1.ForeColor = System.Drawing.Color.Black;
integerTextBox1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
integerTextBox1.Font = new System.Drawing.Font("Arial", 10);
```

---

## Read-Only State Appearance

Create a read-only display mode with visual feedback:

```csharp
// Make read-only and change appearance
integerTextBox1.ReadOnly = true;
integerTextBox1.ReadOnlyBackColor = System.Drawing.Color.AliceBlue;
integerTextBox1.ForeColor = System.Drawing.Color.DimGray;
```

**Combined Read-Only Example:**

```csharp
private void MakeReadOnly()
{
    integerTextBox1.ReadOnly = true;
    integerTextBox1.ReadOnlyBackColor = System.Drawing.Color.LightGray;
    integerTextBox1.PositiveColor = System.Drawing.Color.DarkSlateGray;
    integerTextBox1.NegativeColor = System.Drawing.Color.DarkSlateGray;
    integerTextBox1.ZeroColor = System.Drawing.Color.DarkSlateGray;
}

private void MakeEditable()
{
    integerTextBox1.ReadOnly = false;
    integerTextBox1.BackColor = System.Drawing.Color.White;
    integerTextBox1.PositiveColor = System.Drawing.Color.DarkGreen;
    integerTextBox1.NegativeColor = System.Drawing.Color.Crimson;
    integerTextBox1.ZeroColor = System.Drawing.Color.Gray;
}
```

---

## Color Reset Methods

Reset individual colors to their default values:

```csharp
// Reset background to default
integerTextBox1.ResetBackColor();

// Reset read-only background to default
integerTextBox1.ResetReadOnlyBackColor();

// Reset foreground color to default
integerTextBox1.ResetForeColor();

// Reset positive color to default
integerTextBox1.ResetPositiveColor();
```

---

## Complete Appearance Example

Here's a comprehensive styling example:

```csharp
private void ApplyProfessionalStyle(IntegerTextBox textBox)
{
    // Background
    textBox.BackColor = System.Drawing.Color.White;
    textBox.ReadOnlyBackColor = System.Drawing.Color.WhiteSmoke;
    
    // Text colors
    textBox.PositiveColor = System.Drawing.Color.DarkGreen;
    textBox.NegativeColor = System.Drawing.Color.Crimson;
    textBox.ZeroColor = System.Drawing.Color.Gray;
    
    // Font
    textBox.Font = new System.Drawing.Font("Segoe UI", 10);
    
    // Border
    textBox.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
}
```

---

## Key Takeaways

- Use **BackColor** for normal state and **ReadOnlyBackColor** for read-only state
- Apply **PositiveColor**, **NegativeColor**, **ZeroColor** for value-based coloring
- Use **ForeColor** for general text color
- Combine colors for clear visual feedback
- Reset colors with **Reset*** methods to default
- Value-based coloring helps users quickly identify data status at a glance
