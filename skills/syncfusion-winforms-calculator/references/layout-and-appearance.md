# Layout and Appearance

## Table of Contents
- [Layout Modes](#layout-modes)
- [Background Settings](#background-settings)
- [Border Styles](#border-styles)
- [Button Spacing](#button-spacing)
- [Display Text Alignment](#display-text-alignment)

## Layout Modes

The Calculator control supports two distinct layout modes that determine the button arrangement and visual organization:

### Windows Standard Mode (Default)

The default layout with a standard calculator appearance. Best for general-purpose calculator applications.

```csharp
this.calculatorControl1.LayoutType = Syncfusion.Windows.Forms.Tools.CalculatorLayoutTypes.WindowsStandard;
```

```vb
Me.calculatorControl1.LayoutType = Syncfusion.Windows.Forms.Tools.CalculatorLayoutTypes.WindowsStandard
```

**Layout includes:**
- Numeric keypad (0-9)
- Basic operations (+, -, *, /)
- Memory functions (MS, MR, M+, MC)
- Special operations (%, sqrt, reciprocal)
- Clear and backspace

### Financial Mode

Alternative layout designed for financial calculations with additional features organized differently.

```csharp
this.calculatorControl1.LayoutType = Syncfusion.Windows.Forms.Tools.CalculatorLayoutTypes.Financial;
```

```vb
Me.calculatorControl1.LayoutType = Syncfusion.Windows.Forms.Tools.CalculatorLayoutTypes.Financial
```

**Best used for:**
- Accounting applications
- Financial calculation tools
- Professional calculator interfaces

## Background Settings

Customize the calculator's background appearance using color or images.

### Background Color

Set a solid background color using the BackColor property:

```csharp
this.calculatorControl1.BackColor = System.Drawing.Color.WhiteSmoke;
```

```vb
Me.calculatorControl1.BackColor = System.Drawing.Color.WhiteSmoke
```

### Gradient Background

Create a gradient background effect using the BackgroundColor property with BrushInfo:

```csharp
this.calculatorControl1.BackgroundColor = new Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.Vertical, 
    System.Drawing.Color.WhiteSmoke, 
    System.Drawing.Color.SlateGray
);
```

```vb
Me.calculatorControl1.BackgroundColor = New Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.Vertical, 
    System.Drawing.Color.WhiteSmoke, 
    System.Drawing.Color.SlateGray)
```

### Background Image

Set a background image for the calculator:

```csharp
this.calculatorControl1.BackgroundImage = ((System.Drawing.Image)(resources.GetObject("calculatorControl1.BackgroundImage")));
this.calculatorControl1.BackgroundImageLayout = System.Windows.Forms.ImageLayout.Center;
```

```vb
Me.calculatorControl1.BackgroundImage = DirectCast((resources.GetObject("calculatorControl1.BackgroundImage")), System.Drawing.Image)
Me.calculatorControl1.BackgroundImageLayout = System.Windows.Forms.ImageLayout.Center
```

## Border Styles

The BorderStyle property controls the calculator's border appearance. Use Border3DStyle enum values:

```csharp
this.calculatorControl1.BorderStyle = System.Windows.Forms.Border3DStyle.Etched;
```

```vb
Me.calculatorControl1.BorderStyle = System.Windows.Forms.Border3DStyle.Etched
```

**Available Border Styles:**
- Etched — Engraved/beveled appearance
- Flat — Single-line border
- Raised — Outward 3D effect
- RaisedInner/RaisedOuter — Variations of raised effect
- SunkenInner/SunkenOuter — Variations of sunken effect

## Button Spacing

Control the spacing between calculator buttons for better visual organization.

### Enable Custom Spacing

By default, buttons have minimal spacing. Enable custom spacing with:

```csharp
this.calculatorControl1.UseVerticalAndHorizontalSpacing = true;
this.calculatorControl1.HorizontalSpacing = 5;
this.calculatorControl1.VerticalSpacing = 5;
```

```vb
Me.calculatorControl1.UseVerticalAndHorizontalSpacing = True
Me.calculatorControl1.HorizontalSpacing = 5
Me.calculatorControl1.VerticalSpacing = 5
```

**Spacing Units:** Pixels between buttons
- **HorizontalSpacing:** Space between buttons left-to-right
- **VerticalSpacing:** Space between buttons top-to-bottom

### Button Font and Color

Customize individual button appearance using CalcActions enum to identify specific buttons:

```csharp
// Change Backspace button color
this.calculatorControl1.SetButtonColor(CalcActions.CalcSpecialBackspace, System.Drawing.Color.Black);

// Change Backspace button font
this.calculatorControl1.SetButtonFont(
    CalcActions.CalcSpecialBackspace, 
    new System.Drawing.Font("Arial", 9, System.Drawing.FontStyle.Bold)
);
```

```vb
' Change Backspace button color
Me.calculatorControl1.SetButtonColor(CalcActions.CalcSpecialBackspace, System.Drawing.Color.Black)

' Change Backspace button font
Me.calculatorControl1.SetButtonFont(
    CalcActions.CalcSpecialBackspace, 
    New System.Drawing.Font("Arial", 9, System.Drawing.FontStyle.Bold))
```

**Common CalcActions:**
- CalcOperatorPlus, CalcOperatorMinus, CalcOperatorMultiply, CalcOperatorDivide
- CalcSpecialBackspace, CalcSpecialClear, CalcSpecialClearAll
- CalcMemoryStore (MS), CalcMemoryRecall (MR), CalcMemoryAdd (M+), CalcMemoryClear (MC)
- CalcOperatorEquals (=)

## Display Text Alignment

Control how the display area shows the calculation value.

### Alignment Options

```csharp
// Right-aligned (typical calculator display)
this.calculatorControl1.DisplayTextAlign = System.Windows.Forms.HorizontalAlignment.Right;

// Left-aligned
this.calculatorControl1.DisplayTextAlign = System.Windows.Forms.HorizontalAlignment.Left;

// Center-aligned
this.calculatorControl1.DisplayTextAlign = System.Windows.Forms.HorizontalAlignment.Center;
```

```vb
' Right-aligned (typical calculator display)
Me.calculatorControl1.DisplayTextAlign = System.Windows.Forms.HorizontalAlignment.Right

' Left-aligned
Me.calculatorControl1.DisplayTextAlign = System.Windows.Forms.HorizontalAlignment.Left

' Center-aligned
Me.calculatorControl1.DisplayTextAlign = System.Windows.Forms.HorizontalAlignment.Center
```

### Display Font

Customize the font of the display text area:

```csharp
this.calculatorControl1.Font = new System.Drawing.Font("Verdana", 12F, System.Drawing.FontStyle.Bold);
```

```vb
Me.calculatorControl1.Font = New System.Drawing.Font("Verdana", 12F, System.Drawing.FontStyle.Bold)
```

## Complete Customization Example

```csharp
CalculatorControl calc = new CalculatorControl();
calc.Size = new System.Drawing.Size(320, 300);

// Layout and appearance
calc.LayoutType = CalculatorLayoutTypes.Financial;
calc.BackColor = System.Drawing.Color.LightGray;
calc.BorderStyle = System.Windows.Forms.Border3DStyle.Etched;

// Button spacing
calc.UseVerticalAndHorizontalSpacing = true;
calc.HorizontalSpacing = 4;
calc.VerticalSpacing = 4;

// Display customization
calc.DisplayTextAlign = System.Windows.Forms.HorizontalAlignment.Right;
calc.Font = new System.Drawing.Font("Courier New", 11F, System.Drawing.FontStyle.Bold);

// Button styling
calc.SetButtonColor(CalcActions.CalcOperatorEquals, System.Drawing.Color.Green);
calc.SetButtonFont(CalcActions.CalcOperatorEquals, new System.Drawing.Font("Arial", 10, System.Drawing.FontStyle.Bold));

this.Controls.Add(calc);
```
