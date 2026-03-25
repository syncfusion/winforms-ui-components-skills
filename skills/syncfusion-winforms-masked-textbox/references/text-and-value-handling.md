# Text and Value Handling

## Table of Contents
- [Overview](#overview)
- [Text Property](#text-property)
- [Value Property](#value-property)
- [Comparing Text vs Value](#comparing-text-vs-value)
- [Setting Input Data](#setting-input-data)
- [DataGroups Configuration](#datagroups-configuration)
- [Separator Settings](#separator-settings)
- [Input Validation](#input-validation)

## Overview

The MaskedEditBox provides two primary properties for accessing input data:
- **Text** - Includes mask literal characters (formatted output)
- **Value** - Contains only mask characters, excluding literals (raw data)

Understanding the difference is crucial for proper data handling in your application.

## Text Property

The **Text** property returns the complete input **including literal characters** defined in the mask.

### Behavior

When mask is `(###) ###-####` and user enters `1234567890`:
```csharp
maskedEditBox.Text;  // Result: "(123) 456-7890"
```

### Use Cases

**Use Text when:**
- Displaying formatted output to users
- Showing confirmation dialogs with formatted data
- Storing display-formatted data
- Printing or exporting user-visible formats

### Examples

```csharp
// Phone number display
maskedEditBox.Mask = "(###) ###-####";
string displayPhone = maskedEditBox.Text;  // "(123) 456-7890"

// Social Security display
maskedEditBox.Mask = "###-##-####";
string displaySSN = maskedEditBox.Text;    // "123-45-6789"

// Credit card display
maskedEditBox.Mask = "####-####-####-####";
string displayCC = maskedEditBox.Text;     // "1234-5678-9012-3456"
```

### Setting Text Programmatically

```csharp
// Set with formatting (control parses it)
maskedEditBox.Text = "(123) 456-7890";
// Control extracts digits and reformats: "(123) 456-7890"

// Set with digits only (control applies mask)
maskedEditBox.Text = "1234567890";
// Control formats: "(123) 456-7890"

// Clear input
maskedEditBox.Text = "";
```

## Value Property

The **Value** property returns **only the mask characters**, excluding literal characters defined in the mask.

### Behavior

When mask is `(###) ###-####` and user enters `1234567890`:
```csharp
maskedEditBox.Value;  // Result: "1234567890"
```

### Use Cases

**Use Value when:**
- Saving to database (store only data)
- Transferring data via APIs
- Comparing or validating actual data
- Avoiding storage of formatting characters

### Examples

```csharp
// Store phone in database
maskedEditBox.Mask = "(###) ###-####";
string phoneForDB = maskedEditBox.Value;  // "1234567890" (no formatting)
database.SavePhone(phoneForDB);

// API transmission
string dataToSend = maskedEditBox.Value;  // Clean data
apiClient.SendData(dataToSend);

// Validation
if (maskedEditBox.Value.Length == 10)
{
    // Valid 10-digit phone number
}
```

### Setting Value Programmatically

```csharp
// Set with digits only (control applies mask)
maskedEditBox.Value = "1234567890";
// Control formats: "(123) 456-7890"

// Clear input
maskedEditBox.Value = "";
```

## Comparing Text vs Value

### Side-by-Side Comparison

```csharp
maskedEditBox.Mask = "(###) ###-####";

// User enters: 5551234567

maskedEditBox.Text;   // "(555) 123-4567" (with mask literals)
maskedEditBox.Value;  // "5551234567"    (digits only)

// Length differences
maskedEditBox.Text.Length;    // 14 characters
maskedEditBox.Value.Length;   // 10 characters
```

### Decision Matrix

| Scenario | Use Text | Use Value |
|----------|----------|-----------|
| Display to user | ✓ | ✗ |
| Store in database | ✗ | ✓ |
| Send to API | ✗ | ✓ |
| Validate length | ✗ | ✓ |
| Validate format | ✓ | ✗ |
| Print/Export | ✓ | ✗ |
| Compare values | ✗ | ✓ |

### Practical Example

```csharp
private void SavePhoneNumber()
{
    // Display with formatting
    MessageBox.Show("Phone entered: " + maskedEditBox.Text);  // "(555) 123-4567"
    
    // Store without formatting
    string phoneData = maskedEditBox.Value;  // "5551234567"
    
    // Validate
    if (phoneData.Length != 10)
    {
        MessageBox.Show("Invalid phone number");
        return;
    }
    
    // Save to database
    database.SavePhone(phoneData);
}
```

## Setting Input Data

### Programmatically Setting Values

**Option 1: Set with formatted text (control parses)**

```csharp
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.Text = "(555) 123-4567";
// Result: Text="(555) 123-4567", Value="5551234567"
```

**Option 2: Set with digits only (control formats)**

```csharp
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.Value = "5551234567";
// Result: Text="(555) 123-4567", Value="5551234567"
```

**Option 3: Use assignment directly**

```csharp
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.Text = "5551234567";
// Control auto-applies mask: Text="(555) 123-4567"
```

### Loading Data from Database

```csharp
// Retrieve phone from database (stored without formatting)
string phoneFromDB = database.GetPhone(customerId);  // "5551234567"

// Apply mask for display
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.Value = phoneFromDB;
// User sees: "(555) 123-4567"
```

## DataGroups Configuration

DataGroups allow you to split and align the input text into logical groups with custom spacing and alignment.

### Common Use Cases

**Example: Split phone into 3 groups**

```csharp
maskedEditBox.Mask = "###-##-####";  // SSN

// Define groups (optional positions, not characters)
// Group 1: positions 0-2 (###)
// Group 2: positions 3-4 (##)
// Group 3: positions 5-8 (####)
```

### Setting DataGroups in Code

DataGroups are typically configured via:
1. **Designer:** Right-click control → Properties → DataGroups
2. **Code:** (Advanced - requires Collection Editor)

For most scenarios, the default grouping (based on mask) is sufficient.

### DataGroup Properties

If manually configuring (advanced):

```csharp
// Pseudo-code (exact implementation varies)
maskedEditBox.DataGroups.Add(new DataGroup 
{ 
    Alignment = HorizontalAlignment.Left,
    Text = "Group1"
});
```

## Separator Settings

Separators control how different data types are interpreted and formatted. These are regional/contextual separators, not mask literals.

### Supported Separators

| Separator | Property | Default | Purpose |
|-----------|----------|---------|---------|
| Date | `DateSeparator` | `/` | Date component separator |
| Decimal | `DecimalSeparator` | `.` | Decimal point in numbers |
| Thousand | `ThousandSeparator` | `,` | Thousands grouping |
| Time | `TimeSeparator` | `:` | Time component separator |

### Setting Separators

```csharp
// Change decimal separator for European format
maskedEditBox.DecimalSeparator = ',';   // Use comma instead of period

// Change date separator
maskedEditBox.DateSeparator = '-';      // Use dash in dates

// Change thousand separator
maskedEditBox.ThousandSeparator = '.';  // European format
```

### Example: Regional Format Support

```csharp
// US Format
maskedEditBox.DecimalSeparator = '.';
maskedEditBox.ThousandSeparator = ',';

// European Format
maskedEditBox.DecimalSeparator = ',';
maskedEditBox.ThousandSeparator = '.';

// Currency mask
maskedEditBox.Mask = "$###,##0.00";
// US: $1,234.56
// EU: $1.234,56 (with changed separators)
```

## Input Validation

### Detecting Complete Input

```csharp
private bool IsInputComplete()
{
    // Check if all mask positions are filled
    return maskedEditBox.Value.Length == maskedEditBox.Mask.Count(c => c == '#');
}

// Usage
if (IsInputComplete())
{
    // Process complete phone number
    ProcessPhoneNumber(maskedEditBox.Value);
}
```

### Validating Data Range

```csharp
private void ValidatePhoneArea()
{
    if (maskedEditBox.Value.Length >= 3)
    {
        int areaCode = int.Parse(maskedEditBox.Value.Substring(0, 3));
        if (areaCode < 200)
        {
            MessageBox.Show("Area code must be >= 200");
            maskedEditBox.Value = "";
        }
    }
}
```

### Handling Partial Input

```csharp
private void OnTextChanged(object sender, EventArgs e)
{
    // Allow partial input, show validation state
    if (maskedEditBox.Value.Length == 0)
    {
        statusLabel.Text = "Empty";
    }
    else if (maskedEditBox.Value.Length < 10)
    {
        statusLabel.Text = "Incomplete";
    }
    else if (maskedEditBox.Value.Length == 10)
    {
        statusLabel.Text = "Valid";
    }
}
```

### Pre-validation Before Save

```csharp
private bool ValidateAndSave()
{
    // Check for empty input
    if (string.IsNullOrEmpty(maskedEditBox.Value))
    {
        MessageBox.Show("Phone number required");
        return false;
    }
    
    // Check for complete input
    if (maskedEditBox.Value.Length != 10)
    {
        MessageBox.Show("Phone number must be 10 digits");
        return false;
    }
    
    // Additional business logic
    if (!IsValidPhoneNumber(maskedEditBox.Value))
    {
        MessageBox.Show("Invalid phone number");
        return false;
    }
    
    // Save
    database.SavePhone(maskedEditBox.Value);
    return true;
}
```

## Best Practices

1. **Use Value for storage** - Always save `Value`, not `Text` to database
2. **Use Text for display** - Show `Text` to users in UI
3. **Validate Value** - Validate using `Value` for data-only checks
4. **Check Length of Value** - Use `Value.Length` to verify complete input
5. **Set via Value** - When loading from database, use `Value` assignment
6. **Clear with empty string** - Use `maskedEditBox.Text = ""` to clear
