---
name: syncfusion-winforms-percent-textbox
description: Learn to implement Syncfusion WinForms PercentTextBox control for collecting and displaying percentage values with validation, formatting, and data binding. Covers installation, value management, constraints, formatting options, and event handling for robust percentage input forms.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WinForms PercentTextBox

The **PercentTextBox** is a textbox-derived control that enables users to display, enter, and manage percentage values in Windows Forms applications. It provides built-in formatting, validation, and data binding capabilities for percentage inputs.

## When to Use This Skill

Use this skill when you need to:
- Collect percentage values from users in a Windows Forms application
- Display percentage data with consistent formatting
- Validate percentage inputs within min/max bounds
- Bind percentage values to data sources
- Handle value changes and validation errors programmatically
- Customize number formatting for percentages (decimal places, separators)
- Support both percent and decimal representations of the same value

## Component Overview

**PercentTextBox** derives from the Windows Forms TextBox and adds:
- Automatic percentage formatting and validation
- Support for multiple value representations (PercentValue, DoubleValue, BindableValue)
- Min/Max constraint enforcement with validation
- Customizable number formatting (decimal digits, separators)
- Rich event system for value changes and validation
- Null value handling
- Keyboard input control for negative values

## Documentation Navigation Guide

Select a reference based on what you need to accomplish:

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly references
- Adding control via designer and code
- Basic value initialization
- Minimal working example

### Value Management
📄 **Read:** [references/value-management.md](references/value-management.md)
- PercentValue vs DoubleValue properties
- BindableValue and BindablePercentValue for data binding
- DefaultValue property usage
- Value retrieval and conversion patterns

### Constraints and Validation
📄 **Read:** [references/constraints-and-validation.md](references/constraints-and-validation.md)
- Setting minimum and maximum percentage values
- Min/Max enforcement during validation
- Null value handling (AllowNull, NullString, NullFormat)
- ValidationError event handling
- Validation error prevention strategies

### Formatting and Display
📄 **Read:** [references/formatting-and-display.md](references/formatting-and-display.md)
- Customizing decimal digits (PercentDecimalDigits)
- Decimal and group separators
- Group size configuration
- Format customization examples
- Number format patterns

### Behavior and Input
📄 **Read:** [references/behavior-and-input.md](references/behavior-and-input.md)
- Negative value input handling (NegativeInputPendingOnSelectAll)
- ClipText property for unformatted text
- User input processing
- Keyboard behavior control

### Events and Data Binding
📄 **Read:** [references/events-and-data-binding.md](references/events-and-data-binding.md)
- BindablePercentValueChanged event
- BindableValueChanged event
- DoubleValueChanged event
- Data binding to BindablePercentValue
- Event handler implementation patterns
- Responding to value changes

### Properties and API Reference
📄 **Read:** [references/properties-and-api-reference.md](references/properties-and-api-reference.md)
- Complete property reference organized by category
- Essential methods (ResetMaxValue, ResetMinValue, etc.)
- Read-only properties (FormattedText, ClipText)
- Quick lookup guide for common scenarios

## Quick Start Example

### Basic Setup (Designer)
1. Drag `PercentTextBox` from toolbox to form
2. Set `PercentValue` property to initial value (e.g., 50)
3. Set `MaxValue` and `MinValue` for constraints
4. Subscribe to `BindablePercentValueChanged` event to handle changes

### Basic Setup (Code)

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create instance
PercentTextBox percentTextBox1 = new PercentTextBox();
this.Controls.Add(percentTextBox1);

// Set value
percentTextBox1.PercentValue = 50;

// Set constraints
percentTextBox1.MaxValue = 100;
percentTextBox1.MinValue = 0;

// Subscribe to value changed event
percentTextBox1.BindablePercentValueChanged += (sender, e) => 
{
    Console.WriteLine($"Percent: {percentTextBox1.PercentValue}%");
};
```

## Common Patterns

### Pattern 1: Initialize with Default Value and Constraints
```csharp
var percentBox = new PercentTextBox();
percentBox.PercentValue = 25;
percentBox.MinValue = 0;
percentBox.MaxValue = 100;
percentBox.PercentDecimalDigits = 2;
```

### Pattern 2: Bind to Data Source
```csharp
percentTextBox1.DataBindings.Add("BindablePercentValue", dataSource, "PercentageField");
```

### Pattern 3: Handle Validation Errors
```csharp
percentTextBox1.ValidationError += (sender, e) =>
{
    MessageBox.Show($"Invalid input: {e.ErrorMessage}");
};
```

### Pattern 4: Custom Number Format
```csharp
percentTextBox1.PercentDecimalDigits = 3;
percentTextBox1.PercentDecimalSeparator = ".";
percentTextBox1.PercentGroupSeparator = ",";
percentTextBox1.PercentGroupSizes = new int[] { 2 };
```

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `PercentValue` | double | Gets/sets the percentage value (0-100) |
| `DoubleValue` | double | Gets/sets the decimal value (0-1) |
| `BindablePercentValue` | double? | Nullable percent value for data binding |
| `BindableValue` | double? | Nullable decimal value for data binding |
| `MaxValue` | double | Maximum allowed value |
| `MinValue` | double | Minimum allowed value |
| `DefaultValue` | double | Default value when reset |
| `AllowNull` | bool | Allows null/empty input |
| `NullString` | string | Display text when null |
| `PercentDecimalDigits` | int | Number of decimal places |
| `PercentDecimalSeparator` | string | Decimal separator character |
| `PercentGroupSeparator` | string | Thousands separator |
| `PercentGroupSizes` | int[] | Group size for thousands |
| `NegativeInputPendingOnSelectAll` | bool | Allow negative on select-all |

## Common Use Cases

### Use Case 1: Percentage of Total
Collect a percentage that must be between 0-100 and represents a portion of a whole.
- **Reference:** value-management.md + constraints-and-validation.md

### Use Case 2: Discount or Commission Rate
Accept percentage rates for calculations with specific decimal precision.
- **Reference:** formatting-and-display.md + value-management.md

### Use Case 3: Form with Validation
Build a form that prevents invalid percentage entries and shows errors.
- **Reference:** constraints-and-validation.md + events-and-data-binding.md

### Use Case 4: Data Binding Scenario
Bind PercentTextBox to a data model field with automatic synchronization.
- **Reference:** events-and-data-binding.md + value-management.md

---

**Next:** Choose a reference file above based on your current need, or start with [getting-started.md](references/getting-started.md) if new to PercentTextBox.
