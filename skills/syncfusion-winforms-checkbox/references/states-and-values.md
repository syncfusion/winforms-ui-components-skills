# CheckBoxAdv States and Values

This guide covers the state management and value association features of the CheckBoxAdv control.

## Table of Contents
- [CheckBoxAdv States](#checkboxadv-states)
- [Checked vs CheckState Properties](#checked-vs-checkstate-properties)
- [Tristate Mode](#tristate-mode)
- [Associating Integer Values](#associating-integer-values)
- [Associating String Values](#associating-string-values)
- [Using Value Properties](#using-value-properties)
- [Common Patterns](#common-patterns)

## CheckBoxAdv States

The CheckBoxAdv supports three distinct states:

1. **Checked**: The checkbox is selected/marked
2. **Unchecked**: The checkbox is not selected
3. **Indeterminate**: The checkbox is in an intermediate state (typically shown as grayed)

### Setting States with CheckState

The `CheckState` property provides explicit control over all three states:

```csharp
// Set to checked
checkBoxAdv1.CheckState = CheckState.Checked;

// Set to unchecked
checkBoxAdv1.CheckState = CheckState.Unchecked;

// Set to indeterminate
checkBoxAdv1.CheckState = CheckState.Indeterminate;
```

```vb
' Set to checked
checkBoxAdv1.CheckState = CheckState.Checked

' Set to unchecked
checkBoxAdv1.CheckState = CheckState.Unchecked

' Set to indeterminate
checkBoxAdv1.CheckState = CheckState.Indeterminate
```

### Visual Representation

The three states typically appear as:
- **Checked**: Checkbox with checkmark
- **Unchecked**: Empty checkbox
- **Indeterminate**: Checkbox with gray fill or dash

## Checked vs CheckState Properties

The CheckBoxAdv provides two ways to manage state:

### Checked Property (Boolean)

The `Checked` property is simpler and works with two states only:

```csharp
// Set using boolean
checkBoxAdv1.Checked = true;   // Checked
checkBoxAdv1.Checked = false;  // Unchecked

// Reading state
if (checkBoxAdv1.Checked)
{
    // Checkbox is checked
}
```

**Note:** When `CheckState` is `Indeterminate`, the `Checked` property returns `true`.

### CheckState Property (Three States)

The `CheckState` property provides access to all three states:

```csharp
// More explicit control
checkBoxAdv1.CheckState = CheckState.Checked;

// Check current state
switch (checkBoxAdv1.CheckState)
{
    case CheckState.Checked:
        Console.WriteLine("Checked");
        break;
    case CheckState.Unchecked:
        Console.WriteLine("Unchecked");
        break;
    case CheckState.Indeterminate:
        Console.WriteLine("Indeterminate");
        break;
}
```

### When to Use Which Property

**Use `Checked` when:**
- You need simple yes/no logic
- Only two states are relevant
- Working with boolean data binding

**Use `CheckState` when:**
- You need to distinguish indeterminate state
- Implementing "Select All" with partial selection
- Explicitly setting all three states

## Tristate Mode

The `Tristate` property controls whether users can click the checkbox into the indeterminate state:

### Enabling Tristate

```csharp
// Enable tristate mode
checkBoxAdv1.Tristate = true;
```

When `Tristate = true`:
- User clicks cycle through: Unchecked → Checked → Indeterminate → Unchecked
- All three states are accessible via clicking

When `Tristate = false` (default):
- User clicks cycle through: Unchecked → Checked → Unchecked
- Indeterminate state can only be set programmatically

### Tristate Example: Select All

```csharp
CheckBoxAdv selectAllCheckBox = new CheckBoxAdv();
selectAllCheckBox.Text = "Select All";
selectAllCheckBox.Tristate = true;
selectAllCheckBox.CheckState = CheckState.Indeterminate;

selectAllCheckBox.CheckStateChanged += (sender, e) =>
{
    switch (selectAllCheckBox.CheckState)
    {
        case CheckState.Checked:
            // Select all items
            SelectAllItems();
            break;
        case CheckState.Unchecked:
            // Deselect all items
            DeselectAllItems();
            break;
        case CheckState.Indeterminate:
            // Partial selection - do nothing
            break;
    }
};
```

## Associating Integer Values

You can associate custom integer values with each checkbox state:

### Integer Value Properties

```csharp
// Set integer values for each state
checkBoxAdv1.CheckedInt = 1;
checkBoxAdv1.UncheckedInt = 0;
checkBoxAdv1.IndeterminateInt = -1;

// Get the integer value of current state
int currentValue = checkBoxAdv1.IntValue;
```

```vb
' Set integer values for each state
checkBoxAdv1.CheckedInt = 1
checkBoxAdv1.UncheckedInt = 0
checkBoxAdv1.IndeterminateInt = -1

' Get the integer value of current state
Dim currentValue As Integer = checkBoxAdv1.IntValue
```

### Use Case: Status Codes

```csharp
// Map checkbox states to status codes
CheckBoxAdv statusCheckBox = new CheckBoxAdv();
statusCheckBox.Text = "Task Status";
statusCheckBox.Tristate = true;

// Custom status codes
statusCheckBox.CheckedInt = 100;      // Complete
statusCheckBox.UncheckedInt = 0;      // Not Started
statusCheckBox.IndeterminateInt = 50; // In Progress

// Later, get the status code
int statusCode = statusCheckBox.IntValue;
Console.WriteLine($"Status Code: {statusCode}");
```

### IntValue Property

The `IntValue` property gets or sets the checkbox state using integer values:

```csharp
// Set state using integer
checkBoxAdv1.IntValue = 1;  // Sets to checked (if CheckedInt = 1)

// Get integer representation
int value = checkBoxAdv1.IntValue;
```

**Important for Data Binding:**
- IntValue returns the integer associated with the current state
- Used for binding to integer database fields
- Standard values are -1 (indeterminate), 0 (unchecked), 1 (checked)

## Associating String Values

Similar to integer values, you can associate strings with each state:

### String Value Properties

```csharp
// Set string values for each state
checkBoxAdv1.CheckedString = "Enabled";
checkBoxAdv1.UncheckedString = "Disabled";
checkBoxAdv1.IndeterminateString = "Partial";

// Get the string value of current state
string currentValue = checkBoxAdv1.StringValue;
```

```vb
' Set string values for each state
checkBoxAdv1.CheckedString = "Enabled"
checkBoxAdv1.UncheckedString = "Disabled"
checkBoxAdv1.IndeterminateString = "Partial"

' Get the string value of current state
Dim currentValue As String = checkBoxAdv1.StringValue
```

### Use Case: Configuration Settings

```csharp
CheckBoxAdv settingCheckBox = new CheckBoxAdv();
settingCheckBox.Text = "Feature State";

// Associate descriptive strings
settingCheckBox.CheckedString = "Active";
settingCheckBox.UncheckedString = "Inactive";
settingCheckBox.IndeterminateString = "Unknown";

// Display current state as string
MessageBox.Show($"Current state: {settingCheckBox.StringValue}");
```

## Using Value Properties

The CheckBoxAdv provides three value properties for different data types:

### BoolValue Property

```csharp
// Set state using boolean
checkBoxAdv1.BoolValue = true;  // Checked

// Get boolean representation
bool value = checkBoxAdv1.BoolValue;
```

**Use for:**
- Boolean database fields
- Simple true/false logic
- Bit field data binding

### IntValue Property

```csharp
// Set state using integer
checkBoxAdv1.IntValue = 1;

// Get integer representation
int value = checkBoxAdv1.IntValue;
```

**Use for:**
- Integer database fields
- Status codes
- Numeric state representation

### StringValue Property

```csharp
// Get string representation
string value = checkBoxAdv1.StringValue;
```

**Use for:**
- Display purposes
- Logging
- String-based configurations

## Common Patterns

### Pattern 1: Status Indicator

```csharp
CheckBoxAdv taskStatus = new CheckBoxAdv();
taskStatus.Text = "Task Status";
taskStatus.Tristate = true;

// Define status values
taskStatus.CheckedString = "Complete";
taskStatus.UncheckedString = "Not Started";
taskStatus.IndeterminateString = "In Progress";

taskStatus.CheckedInt = 2;
taskStatus.UncheckedInt = 0;
taskStatus.IndeterminateInt = 1;

// Set initial state
taskStatus.CheckState = CheckState.Indeterminate;

// Display current status
label1.Text = taskStatus.StringValue; // "In Progress"
```

### Pattern 2: Conditional Logic Based on State

```csharp
private void ProcessCheckBox(CheckBoxAdv checkBox)
{
    switch (checkBox.CheckState)
    {
        case CheckState.Checked:
            // Enable all features
            EnableFeatures();
            break;
            
        case CheckState.Unchecked:
            // Disable all features
            DisableFeatures();
            break;
            
        case CheckState.Indeterminate:
            // Enable partial features
            EnablePartialFeatures();
            break;
    }
}
```

### Pattern 3: Converting Between State Types

```csharp
// From boolean to CheckState
CheckState ConvertBoolToState(bool value)
{
    return value ? CheckState.Checked : CheckState.Unchecked;
}

// From CheckState to boolean (indeterminate = true)
bool ConvertStateToBool(CheckState state)
{
    return state != CheckState.Unchecked;
}

// From integer to CheckState
CheckState ConvertIntToState(int value)
{
    if (value > 0) return CheckState.Checked;
    if (value < 0) return CheckState.Indeterminate;
    return CheckState.Unchecked;
}
```

### Pattern 4: State Validation

```csharp
private bool ValidateCheckBoxState(CheckBoxAdv checkBox)
{
    // Ensure state is not indeterminate before processing
    if (checkBox.CheckState == CheckState.Indeterminate)
    {
        MessageBox.Show("Please select a definite option.");
        return false;
    }
    
    return true;
}
```
