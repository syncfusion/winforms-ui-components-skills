# Behavior Modes

The MaskedEditBox control supports three distinct modes that control how the mask is interpreted and applied: **ClipMode**, **InputMode**, and **UsageMode**.

## Overview

Each mode serves a different purpose:
- **ClipMode** - Controls how output is formatted when the mask is removed
- **InputMode** - Controls how user input behaves during entry
- **UsageMode** - Determines the primary use case (visual/data/strict)

## ClipMode

ClipMode controls **what characters are included** when you retrieve the text or value from the control.

### ClipModes Options

| Option | Behavior | Example |
|--------|----------|---------|
| `IncludeLiterals` | Includes mask literal characters in output | `"(123) 456-7890"` |
| `ExcludeLiterals` | Removes literal characters from output | `"1234567890"` |

### ClipMode Examples

**IncludeLiterals (Default):**
```csharp
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.ClipMode = ClipModes.IncludeLiterals;

// User enters: 1234567890
maskedEditBox.Text;   // Result: "(123) 456-7890"
maskedEditBox.Value;  // Result: "(123) 456-7890"
```

**ExcludeLiterals:**
```csharp
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.ClipMode = ClipModes.ExcludeLiterals;

// User enters: 1234567890
maskedEditBox.Text;   // Result: "1234567890"
maskedEditBox.Value;  // Result: "1234567890"
```

### When to Use Each

**Use IncludeLiterals when:**
- Displaying formatted data to users
- Storing formatted output for display purposes
- You want consistent visual formatting

**Use ExcludeLiterals when:**
- Storing data in a database (without formatting)
- Transferring data between systems
- You need only the actual data values
- Reducing data payload in APIs

## InputMode

InputMode controls **how users interact** with the mask during data entry.

### InputModes Options

| Option | Behavior |
|--------|----------|
| `IgnoreSeparators` | User input doesn't include literal separators; control auto-fills them |
| `IncludeSeparators` | User must type literal separators as part of input |

### InputMode Examples

**IgnoreSeparators (Recommended):**
```csharp
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.InputMode = InputModes.IgnoreSeparators;

// User types only digits: 1234567890
// Display automatically becomes: (123) 456-7890
```

**IncludeSeparators:**
```csharp
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.InputMode = InputModes.IncludeSeparators;

// User must type: (1234567890) including parentheses and dashes
// This is more complex and error-prone
```

### When to Use Each

**Use IgnoreSeparators when:**
- You want user experience to be simple (only type data)
- The control automatically handles formatting
- Most general-purpose scenarios (phone, SSN, credit card)

**Use IncludeSeparators when:**
- You need users to understand the exact format
- Educational contexts where format learning is important
- Specialized data entry where format is critical

## UsageMode

UsageMode determines the **primary purpose** of the masked input. This affects how validation and data handling work internally.

### UsageModes Options

| Option | Purpose | Best For |
|--------|---------|----------|
| `Visual` | Format is primarily for display | Phone numbers, formatted IDs, addresses |
| `Data` | Format represents actual data structure | Financial amounts, data codes |
| `Strict` | Enforce complete/valid input only | Validation-critical data entry |

### UsageMode Examples

**Visual Mode (Default):**
```csharp
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.UsageMode = UsageModes.Visual;

// Allows partial input: "(123)"
// Treats formatting as visual guidance
```

**Data Mode:**
```csharp
maskedEditBox.Mask = "###-##-####";  // SSN
maskedEditBox.UsageMode = UsageModes.Data;

// Partial input accepted
// Useful when data has inherent structure
```

**Strict Mode:**
```csharp
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.UsageMode = UsageModes.Strict;

// Requires complete input before accepting
// All mask positions must be filled
```

### When to Use Each

**Use Visual when:**
- Format is mainly for UI consistency
- Partial entries are acceptable
- Examples: phone numbers, formatted IDs, postal codes

**Use Data when:**
- Format represents actual data structure
- Partial entries have meaning
- Examples: structured codes, hierarchical identifiers

**Use Strict when:**
- Complete, valid data is mandatory
- Partial input is invalid
- Examples: credit card numbers, medical IDs, transaction codes

## Combining Modes

Modes work together to create the desired behavior:

### Example 1: User-Friendly Phone Input

```csharp
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.InputMode = InputModes.IgnoreSeparators;   // User types only digits
maskedEditBox.ClipMode = ClipModes.IncludeLiterals;     // Display with formatting
maskedEditBox.UsageMode = UsageModes.Visual;            // Allow partial input

// Result: User types "1234567890", sees "(123) 456-7890", retrieves formatted text
```

### Example 2: Database Storage

```csharp
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.InputMode = InputModes.IgnoreSeparators;   // Easy user input
maskedEditBox.ClipMode = ClipModes.ExcludeLiterals;     // Store only digits
maskedEditBox.UsageMode = UsageModes.Data;              // Treat as data

// Result: User types "1234567890", database stores "1234567890" (no formatting)
```

### Example 3: Strict Credit Card Entry

```csharp
maskedEditBox.Mask = "####-####-####-####";
maskedEditBox.InputMode = InputModes.IgnoreSeparators;   // User types digits
maskedEditBox.ClipMode = ClipModes.ExcludeLiterals;     // Store digits only
maskedEditBox.UsageMode = UsageModes.Strict;            // Require all 16 digits

// Result: User must enter complete 16-digit number; partial input rejected
```

## Mode Selection Guide

**Question 1: How should user input work?**
- Answer: Only digits → `InputMode = IgnoreSeparators`
- Answer: Include separators → `InputMode = IncludeSeparators`

**Question 2: What should be stored/returned?**
- Answer: With formatting → `ClipMode = IncludeLiterals`
- Answer: Digits/letters only → `ClipMode = ExcludeLiterals`

**Question 3: What is the primary purpose?**
- Answer: Visual formatting → `UsageMode = Visual`
- Answer: Structured data → `UsageMode = Data`
- Answer: Strict validation → `UsageMode = Strict`

## Default Behavior

If you don't explicitly set modes, MaskedEditBox uses:
```csharp
maskedEditBox.InputMode = InputModes.IgnoreSeparators;
maskedEditBox.ClipMode = ClipModes.IncludeLiterals;
maskedEditBox.UsageMode = UsageModes.Visual;
```

This provides a balance between user-friendliness and reasonable formatting.

## Performance Considerations

All three modes have minimal performance impact. Choose based on your functional requirements, not performance concerns.

Mode changes are allowed at runtime:
```csharp
void UpdateInputMode(string dataFormat)
{
    maskedEditBox.InputMode = dataFormat == "UserFriendly" 
        ? InputModes.IgnoreSeparators 
        : InputModes.IncludeSeparators;
}
```

However, it's best to set modes during initialization rather than changing them frequently.
