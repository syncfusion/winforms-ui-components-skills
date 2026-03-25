# Getting Started with PercentTextBox

## Table of Contents
- [Installation](#installation)
- [Adding via Designer](#adding-via-designer)
- [Adding via Code](#adding-via-code)
- [Basic Value Setup](#basic-value-setup)
- [Initial Configuration](#initial-configuration)

## Installation

### Assembly References

To use PercentTextBox, add the following assembly to your project:
- **Syncfusion.Shared.Base**
- **Syncfusion.Windows.Forms.Tools** (namespace)

### Adding NuGet Package

```
Install-Package Syncfusion.Shared.Base
```

For more details on NuGet installation: [How to install NuGet packages](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages)

## Adding via Designer

The easiest way to add PercentTextBox to your form:

1. Open Visual Studio and create a new Windows Forms project
2. Locate the **Syncfusion components** in the toolbox
3. Find **PercentTextBox** in the Editors section
4. Drag it onto your form
5. The assembly reference will be added automatically

**Result:** The Syncfusion.Shared.Base assembly is automatically added to your project references.

## Adding via Code

### C# Implementation

Include the required namespace:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

Create and add the control:

```csharp
PercentTextBox percentTextBox1 = new PercentTextBox();
this.Controls.Add(percentTextBox1);
```

### VB.NET Implementation

Include the required namespace:

```vb
Imports Syncfusion.Windows.Forms.Tools
```

Create and add the control:

```vb
Dim percentTextBox1 As PercentTextBox = New PercentTextBox()
Me.Controls.Add(percentTextBox1)
```

## Basic Value Setup

### Setting Initial Percent Value

```csharp
percentTextBox1.PercentValue = 50;  // Sets to 50%
```

The control will display the value with a percent symbol and any configured formatting.

### Setting Position and Size

```csharp
percentTextBox1.Location = new Point(10, 10);
percentTextBox1.Size = new Size(200, 30);
```

## Initial Configuration

### Recommended Starting Configuration

```csharp
// Create instance
PercentTextBox percentTextBox1 = new PercentTextBox();

// Set initial value
percentTextBox1.PercentValue = 0;

// Set min/max bounds
percentTextBox1.MinValue = 0;
percentTextBox1.MaxValue = 100;

// Set decimal precision
percentTextBox1.PercentDecimalDigits = 2;

// Add to form
this.Controls.Add(percentTextBox1);

// Handle value changes
percentTextBox1.BindablePercentValueChanged += (sender, e) =>
{
    // React to value changes
    Console.WriteLine($"New value: {percentTextBox1.PercentValue}");
};
```

### Null Value Handling

If you need to allow empty/null values:

```csharp
percentTextBox1.AllowNull = true;
percentTextBox1.NullString = "No Value";  // Display text when null
```

## Quick Reference

| Property | Purpose | Example |
|----------|---------|---------|
| `PercentValue` | Get/set percentage | `percentTextBox1.PercentValue = 25;` |
| `MinValue` | Minimum allowed value | `percentTextBox1.MinValue = 0;` |
| `MaxValue` | Maximum allowed value | `percentTextBox1.MaxValue = 100;` |
| `PercentDecimalDigits` | Decimal places | `percentTextBox1.PercentDecimalDigits = 2;` |
| `AllowNull` | Allow empty values | `percentTextBox1.AllowNull = true;` |

## Common Issues

**Issue:** Control not appearing in toolbox
- **Solution:** Ensure Syncfusion.Shared.Base assembly is referenced in your project

**Issue:** PercentValue shows as 0 by default
- **Solution:** Set PercentValue explicitly: `percentTextBox1.PercentValue = 50;`

**Issue:** Events not firing
- **Solution:** Ensure you're subscribing to the correct event (BindablePercentValueChanged, not BindableValueChanged if you want percent events)

---

**Next:** Learn about value management in [value-management.md](value-management.md) or set constraints in [constraints-and-validation.md](constraints-and-validation.md)
