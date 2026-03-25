# Value Management and Configuration

## Table of Contents
- [Value Range Properties](#value-range-properties)
- [Change Increments](#change-increments)
- [Increment and Decrement Methods](#increment-and-decrement-methods)
- [Timer Interval](#timer-interval)
- [Value Constraints](#value-constraints)
- [Programmatic Navigation](#programmatic-navigation)

## Value Range Properties

### Minimum Property

Sets the minimum value the slider can reach:

```csharp
trackBarEx1.Minimum = 0;  // Allow values from 0 onwards
```

**Default:** 10

The `Value` property cannot go below this value.

### Maximum Property

Sets the maximum value the slider can reach:

```csharp
trackBarEx1.Maximum = 100;  // Allow values up to 100
```

**Default:** 20

The `Value` property cannot exceed this value.

### Value Property

Gets or sets the current slider position:

```csharp
// Set initial value
trackBarEx1.Value = 50;

// Get current value
int currentValue = trackBarEx1.Value;

// Value must be between Minimum and Maximum
```

### Setting Range and Value

Always set `Minimum` and `Maximum` before setting `Value`:

```csharp
trackBarEx1.Minimum = 0;
trackBarEx1.Maximum = 100;
trackBarEx1.Value = 50;  // Must be within range
```

If you set `Value` outside the range:
- Values below `Minimum` are clamped to `Minimum`
- Values above `Maximum` are clamped to `Maximum`

## Change Increments

### SmallChange Property

Defines the value change for small increments (e.g., arrow keys):

```csharp
trackBarEx1.SmallChange = 1;    // Default: 1
trackBarEx1.SmallChange = 5;    // Larger steps for coarse adjustment
```

**When Used:**
- Arrow key presses
- `SmallIncrease()` and `SmallDecrease()` method calls

### LargeChange Property

Defines the value change for large increments (e.g., Page Up/Page Down):

```csharp
trackBarEx1.LargeChange = 5;    // Default: 5
trackBarEx1.LargeChange = 10;   // Larger steps for faster navigation
```

**When Used:**
- Page Up/Page Down key presses
- `LargeIncrease()` and `LargeDecrease()` method calls

### Setting Appropriate Increments

Choose increments based on your value range:

```csharp
// For 0-100 percentage slider
trackBarEx1.Minimum = 0;
trackBarEx1.Maximum = 100;
trackBarEx1.SmallChange = 1;    // Single percent
trackBarEx1.LargeChange = 10;   // Ten percent

// For 1-10 rating slider
trackBarEx1.Minimum = 1;
trackBarEx1.Maximum = 10;
trackBarEx1.SmallChange = 1;    // One point at a time
trackBarEx1.LargeChange = 3;    // Three points at a time
```

## Increment and Decrement Methods

### SmallIncrease Method

Increases the current value by `SmallChange`:

```csharp
trackBarEx1.SmallIncrease();
// If Value = 45 and SmallChange = 5, Value becomes 50
```

### SmallDecrease Method

Decreases the current value by `SmallChange`:

```csharp
trackBarEx1.SmallDecrease();
// If Value = 45 and SmallChange = 5, Value becomes 40
```

### LargeIncrease Method

Increases the current value by `LargeChange`:

```csharp
trackBarEx1.LargeIncrease();
// If Value = 50 and LargeChange = 10, Value becomes 60
```

### LargeDecrease Method

Decreases the current value by `LargeChange`:

```csharp
trackBarEx1.LargeDecrease();
// If Value = 50 and LargeChange = 10, Value becomes 40
```

### Respecting Boundaries

These methods automatically respect the `Minimum` and `Maximum` values:

```csharp
trackBarEx1.Minimum = 0;
trackBarEx1.Maximum = 100;
trackBarEx1.Value = 98;
trackBarEx1.LargeChange = 10;

trackBarEx1.LargeIncrease();
// Value becomes 100, not 108 (clamped to Maximum)
```

## Timer Interval

### TimerInterval Property

Specifies the delay (in milliseconds) when holding down increment/decrement buttons:

```csharp
trackBarEx1.TimerInterval = 100;   // Default: 100ms
trackBarEx1.TimerInterval = 50;    // Faster increment when holding button
trackBarEx1.TimerInterval = 200;   // Slower increment when holding button
```

**When Used:** When user clicks and holds increment/decrement buttons

### Timer Interval Strategies

```csharp
// Fast increment for large ranges
trackBarEx1.TimerInterval = 50;    // Updates every 50ms

// Slow increment for precise control
trackBarEx1.TimerInterval = 200;   // Updates every 200ms

// Balanced behavior
trackBarEx1.TimerInterval = 100;   // Default, good for most cases
```

## Value Constraints

### Preventing Out-of-Range Values

The control automatically constrains values:

```csharp
trackBarEx1.Minimum = 10;
trackBarEx1.Maximum = 100;

// These assignments are automatically adjusted
trackBarEx1.Value = -50;   // Becomes 10 (Minimum)
trackBarEx1.Value = 150;   // Becomes 100 (Maximum)
trackBarEx1.Value = 50;    // Stays 50 (within range)
```

### Validating Value Before Setting

For safety, validate before assignment:

```csharp
int newValue = GetUserInput();

if (newValue >= trackBarEx1.Minimum && newValue <= trackBarEx1.Maximum)
{
    trackBarEx1.Value = newValue;
}
else
{
    System.Diagnostics.Debug.WriteLine("Value out of range");
}
```

## Programmatic Navigation

### Manual Value Adjustment

```csharp
// Increment by 1
trackBarEx1.Value += 1;

// Decrement by 5
trackBarEx1.Value -= 5;

// Set to specific value
trackBarEx1.Value = 75;

// Reset to minimum
trackBarEx1.Value = trackBarEx1.Minimum;

// Jump to maximum
trackBarEx1.Value = trackBarEx1.Maximum;
```

### Dynamic Range Updates

Update range based on application state:

```csharp
public void UpdateTrackBarRange(int min, int max, int initial)
{
    trackBarEx1.Minimum = min;
    trackBarEx1.Maximum = max;
    trackBarEx1.Value = Math.Max(min, Math.Min(initial, max));
}
```

### Common Scenarios

**Volume Control (0-100):**
```csharp
trackBarEx1.Minimum = 0;
trackBarEx1.Maximum = 100;
trackBarEx1.SmallChange = 1;
trackBarEx1.LargeChange = 10;
trackBarEx1.Value = 50;  // 50% volume
```

**Brightness (0-255):**
```csharp
trackBarEx1.Minimum = 0;
trackBarEx1.Maximum = 255;
trackBarEx1.SmallChange = 1;
trackBarEx1.LargeChange = 20;
trackBarEx1.Value = 128;  // 50% brightness
```

**Zoom Level (10-500%):**
```csharp
trackBarEx1.Minimum = 10;
trackBarEx1.Maximum = 500;
trackBarEx1.SmallChange = 10;
trackBarEx1.LargeChange = 50;
trackBarEx1.Value = 100;  // 100% (normal zoom)
```
