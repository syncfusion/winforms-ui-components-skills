# Keyboard and Runtime Features

## Keyboard Support

The Calculator control supports full keyboard input, allowing users to perform calculations without touching the mouse. All standard calculator buttons have keyboard equivalents mapped to familiar keys and key combinations.

## Keyboard Shortcuts Reference

| Button | Function | Keyboard |
|--------|----------|----------|
| **Backspace** | Delete last digit | BACKSPACE |
| **CE** | Clear entry (current number) | DELETE |
| **C** | Clear all (reset calculation) | ESC |
| **MC** | Memory Clear | CTRL+L |
| **7** | Number seven | 7 |
| **8** | Number eight | 8 |
| **9** | Number nine | 9 |
| **/** | Divide | / |
| **√** | Square root | @ |
| **MR** | Memory Recall | CTRL+R |
| **4** | Number four | 4 |
| **5** | Number five | 5 |
| **6** | Number six | 6 |
| **\*** | Multiply | \* |
| **%** | Percentage | % |
| **MS** | Memory Store | CTRL+M |
| **1** | Number one | 1 |
| **2** | Number two | 2 |
| **3** | Number three | 3 |
| **-** | Subtract | - |
| **1/x** | Reciprocal | R |
| **M+** | Memory Add | CTRL+P |
| **+/-** | Toggle sign | F9 |
| **.** | Decimal point | . or , |
| **+** | Add | + |
| **=** | Equals/Calculate | Enter |

## Numeric Operations

### Basic Arithmetic

Users can perform standard arithmetic operations by entering numbers and operators:

```
5 + 3 =        // Result: 8
10 - 4 =       // Result: 6
7 * 6 =        // Result: 42
20 / 4 =       // Result: 5
```

### Keyboard Example

```
Type: 5
Type: +
Type: 3
Type: Enter (or =)
Display: 8
```

## Memory Operations

The calculator includes memory functions for storing and recalling values during multi-step calculations.

### Memory Functions

| Operation | Keyboard | Description |
|-----------|----------|-------------|
| **MS** | CTRL+M | Memory Store — Save current display value to memory |
| **MR** | CTRL+R | Memory Recall — Display value stored in memory (non-destructive) |
| **M+** | CTRL+P | Memory Add — Add display value to stored memory value |
| **MC** | CTRL+L | Memory Clear — Erase stored memory value |

### Memory Workflow Example

```
Scenario: Calculate (15 + 20) + (8 + 7)

Step 1: Calculate first group
  Type: 15 + 20 =
  Display: 35

Step 2: Store result
  Press: CTRL+M (MS)
  
Step 3: Calculate second group
  Type: 8 + 7 =
  Display: 15

Step 4: Add to memory
  Press: CTRL+P (M+)
  Memory now contains: 50

Step 5: Recall total
  Press: CTRL+R (MR)
  Display: 50
```

## Special Operations

### Square Root

Calculate the square root of the displayed number:

```
Type: 16
Press: @ (or click √ button)
Display: 4
```

### Reciprocal (1/x)

Calculate the reciprocal (inverse) of a number:

```
Type: 4
Press: R (or click 1/x button)
Display: 0.25
```

### Percentage

The percentage operation works in two modes:

**Mode 1: Percentage of a number**
```
Type: 50
Type: *
Type: 25
Type: %
Display: 12.5
(Calculates 25% of 50)
```

**Mode 2: Percentage with operations**
```
Type: 50
Type: +
Type: 25
Type: %
Type: =
Display: 62.5
(Calculates 50 + 25% of 50)
```

### Sign Toggle

Change the sign of the displayed number (positive to negative or vice versa):

```
Type: 42
Press: F9 (or click +/- button)
Display: -42

Press: F9 again
Display: 42
```

### Decimal Point

Enter decimal numbers using the decimal point key:

```
Type: 3.14
Type: *
Type: 2
Type: =
Display: 6.28
```

**Note:** Decimal point character varies by Culture setting. Most locales accept both `.` and `,` as input.

## Clearing Operations

### Clear Entry (CE) — DELETE Key

Clears only the current number being entered, preserving the previous operation:

```
Type: 5 + 3
Press: DELETE
Display: 0 (the 3 is cleared, but the + remains)
Type: 2
Type: =
Display: 7
(Calculates 5 + 2)
```

### Clear All (C) — ESC Key

Resets the entire calculator to initial state:

```
Type: 5 + 3 =
Press: ESC (or click C button)
Display: 0
(All calculations cleared)
```

### Backspace — BACKSPACE Key

Deletes the last digit entered, useful for correcting input errors:

```
Type: 123
Press: BACKSPACE
Display: 12

Press: BACKSPACE again
Display: 1
```

## Runtime Considerations

### Keyboard Focus

The Calculator control captures keyboard input when it has focus. Ensure the calculator has focus for keyboard operations to work:

```csharp
this.calculatorControl1.Focus();  // Give focus to calculator
```

### Repeat Operations

Pressing "=" multiple times repeats the last operation. This behavior depends on the RepeatAssignAction property:

```
Type: 5 + 3 = = =
Display: 11 (first =), 14 (second =), 17 (third =)
(Keeps adding 3)
```

Enable or disable repeat operations:

```csharp
this.calculatorControl1.RepeatAssignAction = true;  // Allow repeats
```

## Practical Keyboard Workflows

### Workflow 1: Quick Calculation at Keyboard

```
CTRL+Home (focus calculator)
5*25=
(Immediately get result: 125)
```

### Workflow 2: Memory-Based Multi-Step Calculation

```
100+50=
CTRL+M                    (Store 150)
75-25=
CTRL+P                    (Add 50 to memory)
CTRL+R                    (Recall total: 200)
```

### Workflow 3: Percentage Calculations

```
200+15%=                  (Result: 230)
ESC                       (Clear)
500-10%=                  (Result: 450)
```

### Workflow 4: Complex Expression with Reciprocals

```
1/(2+3)=
Display: 0.2

Keyboard steps:
Type: 2+3=
Press: R (reciprocal of 5)
Display: 0.2
```

## Troubleshooting

**Issue:** Keyboard shortcuts not working
- **Solution:** Ensure the Calculator control has focus (click on it or call Focus() method)

**Issue:** Different decimal separator than expected
- **Solution:** Check Culture settings; some locales use comma (,) instead of period (.)

**Issue:** Memory operations not storing values
- **Solution:** Verify the value exists before memory operations; memory operations require a valid number in the display

**Issue:** Percentage calculations giving unexpected results
- **Solution:** Review the percentage workflow — operations differ depending on whether % is used mid-calculation or at the end
