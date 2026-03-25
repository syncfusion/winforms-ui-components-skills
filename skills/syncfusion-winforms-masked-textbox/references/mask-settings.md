# Mask Settings

## Table of Contents
- [Overview](#overview)
- [Mask Characters](#mask-characters)
- [Creating Mask Patterns](#creating-mask-patterns)
- [Common Mask Examples](#common-mask-examples)
- [Advanced Patterns](#advanced-patterns)

## Overview

A **mask** defines the format and constraints for input in the MaskedEditBox control. It consists of:
- **Mask characters** (placeholders for user input)
- **Literal characters** (fixed symbols that cannot be changed)

Example: `(###) ###-####`
- `#` = mask characters (user enters digits)
- `()`, space, `-` = literal characters (auto-filled)

## Mask Characters

The MaskedEditBox supports the following mask character types:

| Character | Accepts | Placeholder | Example |
|-----------|---------|-------------|---------|
| `#` | Digits (0-9) only | Digit | `(###) ###-####` for phone |
| `&` | Letters (A-Z, a-z) only | Letter | `&&&&&` for name |
| `$` | Letters or digits | Alphanumeric | `$$$-$$$$` |
| `0-9` | Specific digit | Digit | `0123456789` |
| `:` | Time separator | Auto-filled | Uses system setting |
| `/` | Date separator | Auto-filled | Uses system setting |
| `,` | Thousand separator | Auto-filled | Uses system setting |
| `.` | Decimal separator | Auto-filled | Uses system setting |

### Rules for Mask Characters

- Mask characters define **where** user input is accepted
- Characters outside mask character set are treated as **literals**
- Once defined, mask cannot be changed dynamically during input
- Empty mask (`""`) makes the control behave like a standard TextBox

## Creating Mask Patterns

### Basic Pattern Structure

1. **Identify the format** you need to enforce
2. **Replace user-input positions** with mask characters
3. **Keep literal characters** in their original positions

### Example: Phone Number

Format needed: `(123) 456-7890`
- `(` and `)` = literal (fixed)
- First 3 digits = mask character (`#`)
- space = literal (fixed)
- Next 3 digits = mask character (`#`)
- `-` = literal (fixed)
- Last 4 digits = mask character (`#`)

**Result:** `(###) ###-####`

```csharp
maskedEditBox.Mask = "(###) ###-####";
```

### Example: Social Security Number

Format needed: `123-45-6789`
- 3 digits, dash, 2 digits, dash, 4 digits

**Mask:** `###-##-####`

```csharp
maskedEditBox.Mask = "###-##-####";
```

## Common Mask Examples

### Phone Numbers

**US Format (10 digits):**
```csharp
maskedEditBox.Mask = "(###) ###-####";
// Output: (123) 456-7890
```

**Alternative US Format (with extension):**
```csharp
maskedEditBox.Mask = "(###) ###-#### x####";
// Output: (123) 456-7890 x1234
```

**International Format:**
```csharp
maskedEditBox.Mask = "+1 (###) ###-####";
// Output: +1 (123) 456-7890
```

### IP Address

**IPv4 Format (4 octets):**
```csharp
maskedEditBox.Mask = "###.###.###.###";
// Output: 192.168.001.001
```

### Social Security Number (SSN)

**US Format:**
```csharp
maskedEditBox.Mask = "###-##-####";
// Output: 123-45-6789
```

### Credit Card Number

**Visa/MasterCard (16 digits):**
```csharp
maskedEditBox.Mask = "####-####-####-####";
// Output: 1234-5678-9012-3456
```

**American Express (15 digits):**
```csharp
maskedEditBox.Mask = "####-######-#####";
// Output: 3782-822463-10005
```

### Date Formats

**MM/DD/YYYY:**
```csharp
maskedEditBox.Mask = "##/##/####";
// Output: 12/25/2024
```

**DD/MM/YYYY (European):**
```csharp
maskedEditBox.Mask = "##/##/####";
// Output: 25/12/2024
```

**YYYY-MM-DD (ISO 8601):**
```csharp
maskedEditBox.Mask = "####-##-##";
// Output: 2024-12-25
```

### Time Formats

**HH:MM:SS (24-hour):**
```csharp
maskedEditBox.Mask = "##:##:##";
// Output: 14:30:45
```

**HH:MM (12-hour with AM/PM):**
```csharp
maskedEditBox.Mask = "##:## AM";
// Output: 02:30 PM
```

### Postal Codes

**US Zip Code (5 digits):**
```csharp
maskedEditBox.Mask = "#####";
// Output: 12345
```

**US Zip Code (5+4 format):**
```csharp
maskedEditBox.Mask = "#####-####";
// Output: 12345-6789
```

**Canadian Postal Code (A1A 1A1):**
```csharp
maskedEditBox.Mask = "&# & #";
// Output: K1A 0B1
```

### License Plates

**US Format (3 letters + 3 numbers):**
```csharp
maskedEditBox.Mask = "&&&###";
// Output: ABC123
```

### Product/Part Numbers

**Format: ABC-123-XYZ:**
```csharp
maskedEditBox.Mask = "&&&-###-&&&";
// Output: ABC-123-XYZ
```

## Advanced Patterns

### Numeric Ranges with Validation

For ranges like phone area codes (200-999), use the base mask:

```csharp
maskedEditBox.Mask = "###";  // Accepts 000-999
// Validate in code if specific range needed (e.g., 200-999)
```

Then validate in a `TextChanged` event:

```csharp
private void MaskedEditBox_TextChanged(object sender, EventArgs e)
{
    if (maskedEditBox.Value.Length == 3)
    {
        int areaCode = int.Parse(maskedEditBox.Value);
        if (areaCode < 200 || areaCode > 999)
        {
            MessageBox.Show("Area code must be between 200-999");
            maskedEditBox.Text = "";
        }
    }
}
```

### Optional Sections

Some formats have optional parts. Handle these by:

1. Creating mask for **maximum length** (all fields)
2. Validating optional sections in code

Example: Phone with optional extension

```csharp
maskedEditBox.Mask = "(###) ###-#### x####";
// User can leave extension blank (####)
// Validate: if extension provided, ensure it's 4 digits
```

### Multiple Input Variations

For formats with variations, create separate controls or swap masks dynamically:

```csharp
private void PhoneTypeChanged(string phoneType)
{
    maskedEditBox.Text = "";  // Clear existing input
    
    switch (phoneType)
    {
        case "US":
            maskedEditBox.Mask = "(###) ###-####";
            break;
        case "International":
            maskedEditBox.Mask = "+1 (###) ###-####";
            break;
        case "Extension":
            maskedEditBox.Mask = "(###) ###-#### x####";
            break;
    }
}
```

## Best Practices

1. **Keep masks simple** - Complex masks confuse users; break into multiple fields if needed
2. **Match regional expectations** - Use familiar formats for your target audience
3. **Provide examples** - Show users what format is expected in labels or tooltips
4. **Validate ranges** - Use events to validate ranges (e.g., month 01-12, not 00-99)
5. **Handle optional parts** - Document which sections are optional
6. **Test edge cases** - Verify mask behavior with partial, invalid, and maximum input
