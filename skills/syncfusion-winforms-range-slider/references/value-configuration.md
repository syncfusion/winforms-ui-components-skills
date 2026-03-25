# Value Configuration

## Table of Contents
- [Understanding Range Values](#understanding-range-values)
- [Setting SliderMin and SliderMax](#setting-slidermin-and-slidermax)
- [Configuring Minimum and Maximum Bounds](#configuring-minimum-and-maximum-bounds)
- [Working with Range Property](#working-with-range-property)
- [Practical Examples](#practical-examples)

## Understanding Range Values

The RangeSlider maintains four related value concepts:

| Property | Type | Purpose | Example |
|----------|------|---------|---------|
| `Minimum` | int | Lower bound of selectable range | 0 |
| `Maximum` | int | Upper bound of selectable range | 100 |
| `SliderMin` | int | Current position of left/top thumb | 25 |
| `SliderMax` | int | Current position of right/bottom thumb | 75 |

**Range values are constrained:** SliderMin must be ≥ Minimum and SliderMax must be ≤ Maximum.

## Setting SliderMin and SliderMax

### Individual Thumb Positions

Set each thumb independently to define the selected range:

```csharp
rangeSlider.SliderMin = 20;  // Left thumb position
rangeSlider.SliderMax = 80;  // Right thumb position
```

```vb
rangeSlider.SliderMin = 20  ' Left thumb position
rangeSlider.SliderMax = 80  ' Right thumb position
```

### Properties Detail

**SliderMin Property:**
- Represents the position of the left thumb (horizontal) or top thumb (vertical)
- Must be greater than or equal to `Minimum`
- Must be less than or equal to `SliderMax`
- Defaults to `Minimum` if not set

**SliderMax Property:**
- Represents the position of the right thumb (horizontal) or bottom thumb (vertical)
- Must be less than or equal to `Maximum`
- Must be greater than or equal to `SliderMin`
- Defaults to `Maximum` if not set

### Valid Range Example

```csharp
// Valid configuration
rangeSlider.Minimum = 0;      // Lower bound
rangeSlider.Maximum = 100;    // Upper bound
rangeSlider.SliderMin = 30;   // Valid: >= Minimum (0)
rangeSlider.SliderMax = 70;   // Valid: <= Maximum (100) and >= SliderMin (30)
```

## Configuring Minimum and Maximum Bounds

### Setting Range Bounds

Define the overall selectable range that contains all possible values:

```csharp
rangeSlider.Minimum = 0;
rangeSlider.Maximum = 100;
```

```vb
rangeSlider.Minimum = 0
rangeSlider.Maximum = 100
```

### Properties Detail

**Minimum Property:**
- Specifies the lowest selectable value
- All thumbs must stay at or above this value
- Common values: 0 for 0-100 scale, or domain-specific minimums

**Maximum Property:**
- Specifies the highest selectable value
- All thumbs must stay at or below this value
- Defines the upper limit of the control's range

### Range Scale Examples

**Example 1: Percentage (0-100)**
```csharp
rangeSlider.Minimum = 0;
rangeSlider.Maximum = 100;
```

**Example 2: Price Range ($0-$1000)**
```csharp
rangeSlider.Minimum = 0;        // $0
rangeSlider.Maximum = 1000;     // $1000
rangeSlider.TickFrequency = 100; // $100 intervals
```

**Example 3: Year Range (2000-2030)**
```csharp
rangeSlider.Minimum = 2000;
rangeSlider.Maximum = 2030;
```

**Example 4: Age Range (0-120)**
```csharp
rangeSlider.Minimum = 0;
rangeSlider.Maximum = 120;
```

## Working with Range Property

### Reading the Selected Range

Access the Range property to get the current selection:

```csharp
// Accessing selected range
int minValue = rangeSlider.SliderMin;
int maxValue = rangeSlider.SliderMax;

// Calculate range size
int rangeSize = rangeSlider.SliderMax - rangeSlider.SliderMin;
```

### Range Structure

The Range represents the current selection between the two thumbs:

```csharp
// In value change event
private void rangeSlider_ValueChanged(object sender, EventArgs e)
{
    int selectedMinimum = rangeSlider.SliderMin;
    int selectedMaximum = rangeSlider.SliderMax;
    
    // Update UI or perform filtering
    UpdateFiltersWithRange(selectedMinimum, selectedMaximum);
}
```

## Practical Examples

### Example 1: Price Range Slider

Create a price filter with $50 increments:

```csharp
private void SetupPriceSlider()
{
    rangeSlider.Minimum = 0;           // $0 minimum
    rangeSlider.Maximum = 1000;        // $1000 maximum
    rangeSlider.SliderMin = 100;       // Initial: $100
    rangeSlider.SliderMax = 900;       // Initial: $900
    rangeSlider.TickFrequency = 50;    // $50 intervals
    rangeSlider.ShowTicks = true;
    rangeSlider.ShowLabels = true;
    
    rangeSlider.ValueChanged += (s, e) =>
    {
        decimal minPrice = rangeSlider.SliderMin;
        decimal maxPrice = rangeSlider.SliderMax;
        FilterProductsByPrice(minPrice, maxPrice);
    };
}
```

### Example 2: Age Range Filter

Allow users to filter by age group:

```csharp
private void SetupAgeSlider()
{
    rangeSlider.Minimum = 0;      // Age 0
    rangeSlider.Maximum = 120;    // Age 120
    rangeSlider.SliderMin = 18;   // Initial: 18+
    rangeSlider.SliderMax = 65;   // Initial: 65 max
    rangeSlider.TickFrequency = 5; // 5-year intervals
    rangeSlider.ShowTicks = true;
    rangeSlider.ShowLabels = true;
    
    rangeSlider.ValueChanged += (s, e) =>
    {
        int minAge = rangeSlider.SliderMin;
        int maxAge = rangeSlider.SliderMax;
        FilterUsersByAge(minAge, maxAge);
    };
}
```

### Example 3: Year Range for Date Filtering

Select historical data by year range:

```csharp
private void SetupYearSlider()
{
    int currentYear = DateTime.Now.Year;
    rangeSlider.Minimum = 2000;          // Start year
    rangeSlider.Maximum = currentYear;   // Current year
    rangeSlider.SliderMin = currentYear - 10;  // Last 10 years
    rangeSlider.SliderMax = currentYear;       // To present
    rangeSlider.TickFrequency = 2;       // 2-year intervals
    rangeSlider.ShowTicks = true;
    rangeSlider.ShowLabels = true;
    
    rangeSlider.ValueChanged += (s, e) =>
    {
        int startYear = rangeSlider.SliderMin;
        int endYear = rangeSlider.SliderMax;
        LoadDataForYearRange(startYear, endYear);
    };
}
```

### Example 4: Dynamic Configuration

Configure slider based on data range:

```csharp
public void ConfigureSliderForData(List<int> data)
{
    if (data.Count == 0) return;
    
    int minValue = data.Min();
    int maxValue = data.Max();
    int range = maxValue - minValue;
    
    rangeSlider.Minimum = minValue;
    rangeSlider.Maximum = maxValue;
    
    // Set initial range to show all data
    rangeSlider.SliderMin = minValue;
    rangeSlider.SliderMax = maxValue;
    
    // Calculate appropriate tick frequency
    if (range > 1000)
        rangeSlider.TickFrequency = 100;
    else if (range > 100)
        rangeSlider.TickFrequency = 10;
    else
        rangeSlider.TickFrequency = 1;
}
```

### Example 5: Responsive Range Updates

Update slider configuration when data changes:

```csharp
private void OnDataSourceChanged(List<DataPoint> newData)
{
    // Recalculate bounds
    int newMin = newData.Min(d => d.Value);
    int newMax = newData.Max(d => d.Value);
    
    // Update bounds (preserves thumb positions if still valid)
    rangeSlider.Minimum = newMin;
    rangeSlider.Maximum = newMax;
    
    // Clamp existing positions to new bounds
    if (rangeSlider.SliderMin < newMin)
        rangeSlider.SliderMin = newMin;
    if (rangeSlider.SliderMax > newMax)
        rangeSlider.SliderMax = newMax;
}
```

## Key Constraints

**Value Ordering:**
- Always: `Minimum < Maximum`
- Always: `Minimum ≤ SliderMin ≤ SliderMax ≤ Maximum`
- Violating these constraints may result in automatic value correction

**Setting Order:**
Set bounds (Minimum/Maximum) before setting thumb positions (SliderMin/SliderMax) to avoid constraint violations:

```csharp
// Correct order
rangeSlider.Minimum = 0;        // Set bounds first
rangeSlider.Maximum = 100;
rangeSlider.SliderMin = 25;     // Set positions after
rangeSlider.SliderMax = 75;
```

---

**Related:** Interactive Features | Event Handling | Layout and Orientation
