# Formula Support

## Table of Contents
- [Overview](#overview)
- [Enabling Formulas](#enabling-formulas)
- [Formula Syntax](#formula-syntax)
- [Cell References](#cell-references)
- [Built-in Functions](#built-in-functions)
- [Operators](#operators)
- [Creating Formulas](#creating-formulas)
- [Formula Events](#formula-events)
- [Custom Functions](#custom-functions)
- [Troubleshooting](#troubleshooting)

## Overview

GridControl supports Excel-like formulas that allow algebraic expressions using cell references, built-in functions, and operators. Formulas automatically recalculate when dependent cells change.

## Enabling Formulas

### Enable Formula Engine:

```csharp
// Enable formula support
gridControl1.Model.EnableFormulas = true;

// Optional: Set cell type to FormulaCell (not required if EnableFormulas is true)
gridControl1[2, 2].CellType = GridCellTypeName.FormulaCell;
```

**Note:** When `EnableFormulas` is true, any cell value starting with `=` is treated as a formula.

## Formula Syntax

### Basic Rules:

- Start with equal sign `=`
- Use cell references (A1, B2, C3, etc.)
- Support parentheses for grouping
- Case-insensitive function names

### Examples:

```csharp
// Simple arithmetic
gridControl1[5, 1].CellValue = "=A1+A2";

// With parentheses
gridControl1[5, 2].CellValue = "=(B1+B2)*B3";

// Cell ranges
gridControl1[5, 3].CellValue = "=SUM(C1:C4)";

// Mixed references
gridControl1[5, 4].CellValue = "=A1*B2+C3";
```

## Cell References

### Absolute Cell References:

```
A1, B2, C3 - Column letter + row number
```

### Example Setup:

```csharp
// Data cells
gridControl1[1, 1].CellValue = 10;   // A1
gridControl1[2, 1].CellValue = 20;   // A2
gridControl1[3, 1].CellValue = 30;   // A3
gridControl1[4, 1].CellValue = 40;   // A4

// Formula cells
gridControl1[5, 1].CellValue = "=A1+A2";      // Result: 30
gridControl1[6, 1].CellValue = "=A1*A2";      // Result: 200
gridControl1[7, 1].CellValue = "=SUM(A1:A4)"; // Result: 100
```

## Built-in Functions

### Mathematical Functions:

```csharp
// SUM - Add values
gridControl1[5, 1].CellValue = "=SUM(A1:A4)";

// AVERAGE - Calculate average
gridControl1[5, 2].CellValue = "=AVERAGE(A1:A4)";

// MAX - Find maximum
gridControl1[5, 3].CellValue = "=MAX(A1:A4)";

// MIN - Find minimum
gridControl1[5, 4].CellValue = "=MIN(A1:A4)";

// COUNT - Count numeric values
gridControl1[5, 5].CellValue = "=COUNT(A1:A10)";

// ABS - Absolute value
gridControl1[6, 1].CellValue = "=ABS(A1-A2)";

// SQRT - Square root
gridControl1[6, 2].CellValue = "=SQRT(A1)";

// POWER - Raise to power
gridControl1[6, 3].CellValue = "=POWER(A1,2)";  // A1^2
```

### Statistical Functions:

```csharp
// AVERAGE
gridControl1[7, 1].CellValue = "=AVERAGE(A1:A10)";

// MEDIAN
gridControl1[7, 2].CellValue = "=MEDIAN(A1:A10)";

// STDEV - Standard deviation
gridControl1[7, 3].CellValue = "=STDEV(A1:A10)";
```

### Logical Functions:

```csharp
// IF - Conditional logic
gridControl1[8, 1].CellValue = "=IF(A1>50,\"High\",\"Low\")";

// AND - Logical AND
gridControl1[8, 2].CellValue = "=AND(A1>10,A2<100)";

// OR - Logical OR
gridControl1[8, 3].CellValue = "=OR(A1>100,A2>100)";

// NOT - Logical NOT
gridControl1[8, 4].CellValue = "=NOT(A1>50)";
```

### Text Functions:

```csharp
// CONCAT - Concatenate strings
gridControl1[9, 1].CellValue = "=CONCAT(A1,\" \",B1)";

// UPPER - Convert to uppercase
gridControl1[9, 2].CellValue = "=UPPER(A1)";

// LOWER - Convert to lowercase
gridControl1[9, 3].CellValue = "=LOWER(A1)";

// LEN - String length
gridControl1[9, 4].CellValue = "=LEN(A1)";
```

### Date Functions:

```csharp
// TODAY - Current date
gridControl1[10, 1].CellValue = "=TODAY()";

// NOW - Current date and time
gridControl1[10, 2].CellValue = "=NOW()";

// YEAR - Extract year
gridControl1[10, 3].CellValue = "=YEAR(A1)";

// MONTH - Extract month
gridControl1[10, 4].CellValue = "=MONTH(A1)";

// DAY - Extract day
gridControl1[10, 5].CellValue = "=DAY(A1)";
```

## Operators

### Arithmetic Operators:

```csharp
// Addition
gridControl1[11, 1].CellValue = "=A1+B1";

// Subtraction
gridControl1[11, 2].CellValue = "=A1-B1";

// Multiplication
gridControl1[11, 3].CellValue = "=A1*B1";

// Division
gridControl1[11, 4].CellValue = "=A1/B1";

// Modulus
gridControl1[11, 5].CellValue = "=A1%B1";

// Exponentiation
gridControl1[11, 6].CellValue = "=A1^2";
```

### Comparison Operators:

```csharp
// Greater than
gridControl1[12, 1].CellValue = "=A1>B1";

// Less than
gridControl1[12, 2].CellValue = "=A1<B1";

// Equal to
gridControl1[12, 3].CellValue = "=A1=B1";

// Greater than or equal
gridControl1[12, 4].CellValue = "=A1>=B1";

// Less than or equal
gridControl1[12, 5].CellValue = "=A1<=B1";

// Not equal
gridControl1[12, 6].CellValue = "=A1<>B1";
```

**Operator Precedence:**
1. Multiplication (*), Division (/)
2. Addition (+), Subtraction (-)
3. Comparison (<, >, <=, >=, =, <>)

## Creating Formulas

### Simple Formulas:

```csharp

// Add numeric data
gridControl1[1, 1].CellValue = 100;
gridControl1[2, 1].CellValue = 200;
gridControl1[3, 1].CellValue = 300;

// Add formulas
gridControl1[4, 1].CellValue = "=A1+A2+A3";  // Sum
gridControl1[5, 1].CellValue = "=(A1+A2+A3)/3";  // Average
gridControl1[6, 1].CellValue = "=A3-A1";  // Difference
```

### Complex Formulas:

```csharp
// Nested IF statements
gridControl1[7, 1].CellValue = "=IF(A1>200,\"Large\",IF(A1>100,\"Medium\",\"Small\"))";

// Multiple functions
gridControl1[8, 1].CellValue = "=SUM(A1:A3)*AVERAGE(B1:B3)";

// Conditional sum
gridControl1[9, 1].CellValue = "=IF(A1>100,SUM(A1:A3),0)";
```

### Invoice Calculation Example:

```csharp
// Item prices
gridControl1[1, 1].Text = "Item";
gridControl1[1, 2].Text = "Qty";
gridControl1[1, 3].Text = "Price";
gridControl1[1, 4].Text = "Total";

gridControl1[2, 1].Text = "Widget";
gridControl1[2, 2].CellValue = 10;
gridControl1[2, 3].CellValue = 19.99;
gridControl1[2, 4].CellValue = "=B2*C2";  // Calculate total

gridControl1[3, 1].Text = "Gadget";
gridControl1[3, 2].CellValue = 5;
gridControl1[3, 3].CellValue = 29.99;
gridControl1[3, 4].CellValue = "=B3*C3";

// Subtotal
gridControl1[5, 3].Text = "Subtotal:";
gridControl1[5, 4].CellValue = "=SUM(D2:D3)";

// Tax (8%)
gridControl1[6, 3].Text = "Tax:";
gridControl1[6, 4].CellValue = "=D5*0.08";

// Total
gridControl1[7, 3].Text = "Total:";
gridControl1[7, 4].CellValue = "=D5+D6";
```

## Formula Events

### QueryCellFormula Event:

Customize formula behavior:

```csharp
gridControl1.QueryCellFormula += (sender, e) =>
{
    // Custom formula handling
    if (e.Row == 5 && e.Col == 5)
    {
        e.Formula = "=A1+B1+C1";
        e.Handled = true;
    }
};
```

### FormulaInfoParsed Event:

Triggered after formula is parsed:

```csharp
gridControl1.Model.FormulaInfoParsed += (sender, e) =>
{
    Console.WriteLine($"Formula parsed: {e.Formula}");
};
```

## Custom Functions

Register custom functions for specialized calculations:

```csharp
// Register custom function
gridControl1.Model.FormulaEngine.AddFunction("CUSTOMSUM", new CustomSumFunction());

// Create custom function class
public class CustomSumFunction : IFormulaFunction
{
    public string Compute(string args)
    {
        string[] values = args.Split(',');
        double sum = 0;
        
        foreach (string val in values)
        {
            if (double.TryParse(val, out double num))
            {
                sum += num;
            }
        }
        
        return sum.ToString();
    }
}

// Use custom function
gridControl1[10, 1].CellValue = "=CUSTOMSUM(A1,A2,A3)";
```

## Best Practices

1. **Enable formula engine** before setting formula values
2. **Use cell references** instead of hard-coded values
3. **Validate formula syntax** before assigning
4. **Handle circular references** with caution
5. **Test formulas** with various input values
6. **Document complex formulas** for maintainability
7. **Use named ranges** for clarity (if supported)

## Troubleshooting

### Formulas not calculating
- Verify `Model.EnableFormulas` is true
- Check formula starts with `=`
- Validate cell references are correct
- Ensure referenced cells have values

### #VALUE! Error
- Check for invalid cell references
- Verify data types in formula
- Ensure functions are spelled correctly

### #DIV/0! Error
- Check for division by zero
- Add IF statement to handle zero values

### Circular Reference
- Verify formulas don't reference themselves
- Check for indirect circular dependencies

### Performance Issues
- Limit number of formulas
- Use BeginUpdate/EndUpdate for batch changes
- Consider caching calculated values

## Next Steps

- Implement data validation with formulas
- Create calculated columns
- Build dynamic dashboards
- Add conditional formatting based on formulas
- Export formulas to Excel
