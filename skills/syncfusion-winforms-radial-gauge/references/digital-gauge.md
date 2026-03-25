# DigitalGauge

## Table of Contents
- [Overview](#overview)
- [Getting Started](#getting-started)
- [Character Types](#character-types)
- [Character Count](#character-count)
- [Segment Spacing](#segment-spacing)
- [Invisible Segments](#invisible-segments)
- [Frame Customization](#frame-customization)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

DigitalGauge displays **alphanumeric characters in LED/digital format**, mimicking retro digital displays. Ideal for clocks, counters, status codes, and numeric readouts.

**Key capabilities:**
- 4 character types (DotMatrix, SevenSegment, FourteenSegment, SixteenSegment)
- Configurable character count
- Segment spacing control
- Ghost segment display option
- Rounded corners support
- Professional LED styling

## Getting Started

### Designer Setup

1. **Add DigitalGauge to form:**
   - Drag from Toolbox → Place on form
   - Set Size (e.g., 250x80 for clock display)

2. **Configure basic properties (Properties window):**
   ```
   CharacterType: SevenSegment
   CharacterCount: 8
   SegmentSpacing: 2
   Value: "12:34:56"
   ForeColor: Red
   ShowInvisibleSegments: True
   ```

3. **Run** - LED display shows configured text

### Code Setup

```csharp
using Syncfusion.Windows.Forms.Gauge;

// Create gauge
DigitalGauge ledDisplay = new DigitalGauge();
ledDisplay.Size = new Size(250, 80);
ledDisplay.Location = new Point(20, 20);

// Basic configuration
ledDisplay.CharacterType = CharacterType.SevenSegment;
ledDisplay.CharacterCount = 8;
ledDisplay.SegmentSpacing = 2.0f;
ledDisplay.Value = "12:34:56";
ledDisplay.ForeColor = Color.Red;
ledDisplay.BackColor = Color.Black;
ledDisplay.ShowInvisibleSegments = true;

// Add to form
this.Controls.Add(ledDisplay);
```

## Character Types

### SevenSegment

Classic 7-segment LED display (numbers 0-9, limited letters).

```csharp
DigitalGauge gauge = new DigitalGauge();
gauge.CharacterType = CharacterType.SevenSegment;
gauge.Value = "1234567890";
```

**Supported characters:**
- **Numbers:** 0-9 ✓
- **Letters:** A, B, C, D, E, F, H, L, O, P, U (uppercase only, limited)
- **Symbols:** Dash (-), Space

**Use when:**
- Numeric displays only
- Classic digital clock style
- Retro LED appearance
- Minimal character set needed

**Display characteristics:**
- 7 segments per character
- Best for numbers
- Limited letter support
- Most compact representation

### FourteenSegment

Enhanced 14-segment display (alphanumeric).

```csharp
DigitalGauge gauge = new DigitalGauge();
gauge.CharacterType = CharacterType.FourteenSegment;
gauge.Value = "HELLO WORLD";
```

**Supported characters:**
- **Numbers:** 0-9 ✓
- **Letters:** A-Z (uppercase), a-z (lowercase) ✓
- **Symbols:** Most common symbols ✓

**Use when:**
- Alphanumeric text needed
- Status messages
- Product codes
- Mixed text and numbers

**Display characteristics:**
- 14 segments per character
- Good letter rendering
- Balanced legibility vs complexity
- Moderate segment density

### SixteenSegment

Full 16-segment display (complete alphanumeric + symbols).

```csharp
DigitalGauge gauge = new DigitalGauge();
gauge.CharacterType = CharacterType.SixteenSegment;
gauge.Value = "Test: 99%";
```

**Supported characters:**
- **Numbers:** 0-9 ✓
- **Letters:** A-Z, a-z (excellent rendering) ✓
- **Symbols:** Full symbol set ✓

**Use when:**
- Complex text display needed
- Maximum character variety
- High-quality letter rendering
- Professional appearance

**Display characteristics:**
- 16 segments per character
- Best letter quality
- Supports most symbols
- Highest segment density

### DotMatrix

5x7 dot matrix display (pixel-based).

```csharp
DigitalGauge gauge = new DigitalGauge();
gauge.CharacterType = CharacterType.DotMatrix;
gauge.Value = "Matrix Style";
```

**Supported characters:**
- **Numbers:** 0-9 ✓
- **Letters:** A-Z, a-z ✓
- **Symbols:** Full ASCII set ✓

**Use when:**
- Pixel-art aesthetic desired
- Scrolling text displays
- Matrix/terminal style
- Maximum character flexibility

**Display characteristics:**
- 5x7 dot grid per character
- Pixel-based rendering
- Most flexible character support
- Retro computing style

## Character Count

Controls how many characters are displayed.

### Setting Character Count

```csharp
// Display 8 characters
gauge.CharacterCount = 8;
gauge.Value = "12345678";  // All 8 displayed
```

### Value Truncation

When value exceeds character count, display is truncated.

```csharp
gauge.CharacterCount = 4;
gauge.Value = "123456";  // Displays "1234" only

// Right-aligned truncation (shows rightmost characters)
gauge.Value = "ABCDEF";  // Might display "CDEF" depending on implementation
```

### Dynamic Character Count

```csharp
// Adjust character count based on content
private void SetDisplayValue(string text)
{
    gauge.CharacterCount = Math.Max(text.Length, 4);  // Minimum 4 chars
    gauge.Value = text;
    
    // Resize gauge based on character count
    int charWidth = 25;  // Approximate width per character
    gauge.Width = gauge.CharacterCount * charWidth + 20;  // +20 for padding
}
```

### Common Character Count Configurations

```csharp
// Digital clock (HH:MM:SS)
gauge.CharacterCount = 8;

// Date display (MM/DD/YYYY)
gauge.CharacterCount = 10;

// Counter (0000)
gauge.CharacterCount = 4;

// Temperature (72.5°F)
gauge.CharacterCount = 6;

// Price display ($99.99)
gauge.CharacterCount = 6;
```

## Segment Spacing

Controls spacing between individual characters.

### Setting Spacing

```csharp
// Compact spacing
gauge.SegmentSpacing = 1.0f;

// Default spacing
gauge.SegmentSpacing = 2.0f;

// Wide spacing
gauge.SegmentSpacing = 5.0f;
```

**Spacing values:**
- `0.5f - 1.5f` - Compact (tightly packed)
- `2.0f - 3.0f` - Normal (readable)
- `4.0f - 6.0f` - Wide (clear separation)
- `> 6.0f` - Very wide (individual digits emphasized)

### Spacing for Different Scenarios

```csharp
// Clock display - readable spacing
clockGauge.SegmentSpacing = 2.5f;

// Counter - compact spacing
counterGauge.SegmentSpacing = 1.0f;

// Status message - clear spacing
statusGauge.SegmentSpacing = 3.0f;

// Price display - moderate spacing
priceGauge.SegmentSpacing = 2.0f;
```

### Dynamic Spacing Adjustment

```csharp
// Adjust spacing based on available width
private void AdjustSpacing(int targetWidth)
{
    int charCount = gauge.CharacterCount;
    float availableSpace = targetWidth - (charCount * 20);  // 20 = min char width
    float spacing = availableSpace / (charCount - 1);
    
    gauge.SegmentSpacing = Math.Max(1.0f, Math.Min(spacing, 6.0f));
}
```

## Invisible Segments

Shows inactive segments in "ghost" appearance.

### Enabling Ghost Segments

```csharp
// Show all possible segments dimmed
gauge.ShowInvisibleSegments = true;

// Example: Shows "888" pattern when displaying "123"
gauge.CharacterType = CharacterType.SevenSegment;
gauge.Value = "123";
gauge.ShowInvisibleSegments = true;  // Inactive segments visible
```

**Visual effect:**
- Active segments: Full brightness (ForeColor)
- Inactive segments: Dimmed appearance (typically 10-20% opacity)

### Styling Ghost Segments

```csharp
// Bright active segments
gauge.ForeColor = Color.Red;
gauge.BackColor = Color.Black;
gauge.ShowInvisibleSegments = true;

// Result: Red active segments, dark red ghost segments
```

### When to Use Ghost Segments

**Enable (`true`) when:**
- Digital clock displays (shows full character structure)
- Retro LED aesthetic desired
- User needs to see character boundaries
- Classic calculator/watch style

**Disable (`false`) when:**
- Clean minimal look desired
- Status messages (text-only display)
- Modern UI design
- Reduce visual clutter

### Example Comparison

```csharp
// Without ghost segments
gauge1.Value = "12:34";
gauge1.ShowInvisibleSegments = false;
// Displays: "12:34" with black background

// With ghost segments
gauge2.Value = "12:34";
gauge2.ShowInvisibleSegments = true;
// Displays: "88:88" pattern with "12:34" bright, rest dimmed
```

## Frame Customization

### Colors

```csharp
// Active segment color
gauge.ForeColor = Color.LimeGreen;

// Background color
gauge.BackColor = Color.Black;

// Classic LED combinations
gauge.ForeColor = Color.Red;     // Red LEDs
gauge.ForeColor = Color.Lime;    // Green LEDs
gauge.ForeColor = Color.Cyan;    // Cyan LEDs
gauge.ForeColor = Color.Orange;  // Amber LEDs
```

### Rounded Corners

```csharp
// Sharp corners (default)
gauge.RoundCornerRadius = 0;

// Slightly rounded
gauge.RoundCornerRadius = 5;

// Heavily rounded
gauge.RoundCornerRadius = 15;

// Maximum rounding
gauge.RoundCornerRadius = 25;
```

**RoundCornerRadius range:** 0-50
- `0` - Sharp rectangular frame
- `5-10` - Subtle rounding (modern)
- `15-25` - Noticeable rounding (friendly)
- `> 25` - Very rounded (button-like)

### Frame Size

```csharp
// Adjust size based on character count
int charCount = 8;
int charWidth = 30;  // Approximate width per char
int height = 80;     // Standard height

gauge.Size = new Size(charCount * charWidth, height);
```

### Border and Padding

```csharp
// Add border for framed look
gauge.BorderStyle = BorderStyle.FixedSingle;

// Or use panel with border
Panel frame = new Panel();
frame.BorderStyle = BorderStyle.Fixed3D;
frame.Padding = new Padding(5);
frame.BackColor = Color.DarkGray;
gauge.Dock = DockStyle.Fill;
frame.Controls.Add(gauge);
```

## Common Scenarios

### Scenario 1: Digital Clock

```csharp
DigitalGauge clock = new DigitalGauge();
clock.Size = new Size(280, 90);
clock.Location = new Point(20, 20);
clock.CharacterType = CharacterType.SevenSegment;
clock.CharacterCount = 8;
clock.SegmentSpacing = 3.0f;
clock.ForeColor = Color.Red;
clock.BackColor = Color.Black;
clock.ShowInvisibleSegments = true;
clock.RoundCornerRadius = 8;
clock.Value = DateTime.Now.ToString("HH:mm:ss");

// Update every second
Timer timer = new Timer();
timer.Interval = 1000;
timer.Tick += (s, e) => {
    clock.Value = DateTime.Now.ToString("HH:mm:ss");
};
timer.Start();

this.Controls.Add(clock);
```

### Scenario 2: Counter/Score Display

```csharp
DigitalGauge counter = new DigitalGauge();
counter.Size = new Size(180, 70);
counter.CharacterType = CharacterType.SevenSegment;
counter.CharacterCount = 6;
counter.SegmentSpacing = 2.0f;
counter.ForeColor = Color.Lime;
counter.BackColor = Color.Black;
counter.ShowInvisibleSegments = false;
counter.Value = "000000";

// Increment counter
int count = 0;
void IncrementCounter()
{
    count++;
    counter.Value = count.ToString("D6");  // 6-digit format with leading zeros
}

this.Controls.Add(counter);
```

### Scenario 3: Status Message Display

```csharp
DigitalGauge status = new DigitalGauge();
status.Size = new Size(400, 60);
status.CharacterType = CharacterType.FourteenSegment;
status.CharacterCount = 20;
status.SegmentSpacing = 1.5f;
status.ForeColor = Color.Orange;
status.BackColor = Color.DarkSlateGray;
status.ShowInvisibleSegments = false;
status.Value = "SYSTEM READY";

// Update status
void SetStatus(string message)
{
    status.Value = message.PadRight(status.CharacterCount).Substring(0, gauge.CharacterCount);
}

this.Controls.Add(status);
```

### Scenario 4: Temperature Display

```csharp
DigitalGauge tempDisplay = new DigitalGauge();
tempDisplay.Size = new Size(150, 60);
tempDisplay.CharacterType = CharacterType.SevenSegment;
tempDisplay.CharacterCount = 5;
tempDisplay.SegmentSpacing = 2.5f;
tempDisplay.ForeColor = Color.Cyan;
tempDisplay.BackColor = Color.Navy;
tempDisplay.ShowInvisibleSegments = true;
tempDisplay.Value = "72.5F";

// Update temperature
void UpdateTemperature(float temp)
{
    tempDisplay.Value = temp.ToString("F1") + "F";
}

this.Controls.Add(tempDisplay);
```

### Scenario 5: Price Display

```csharp
DigitalGauge priceDisplay = new DigitalGauge();
priceDisplay.Size = new Size(200, 80);
priceDisplay.CharacterType = CharacterType.SevenSegment;
priceDisplay.CharacterCount = 7;
priceDisplay.SegmentSpacing = 2.0f;
priceDisplay.ForeColor = Color.Yellow;
priceDisplay.BackColor = Color.Black;
priceDisplay.ShowInvisibleSegments = true;
priceDisplay.RoundCornerRadius = 10;
priceDisplay.Value = "$" + "99.99";

// Update price
void SetPrice(decimal price)
{
    priceDisplay.Value = "$" + price.ToString("F2");
}

this.Controls.Add(priceDisplay);
```

### Scenario 6: Stopwatch

```csharp
DigitalGauge stopwatch = new DigitalGauge();
stopwatch.Size = new Size(250, 80);
stopwatch.CharacterType = CharacterType.SevenSegment;
stopwatch.CharacterCount = 8;
stopwatch.SegmentSpacing = 2.5f;
stopwatch.ForeColor = Color.LimeGreen;
stopwatch.BackColor = Color.Black;
stopwatch.ShowInvisibleSegments = true;

System.Diagnostics.Stopwatch sw = new System.Diagnostics.Stopwatch();

Timer updateTimer = new Timer();
updateTimer.Interval = 100;  // Update every 100ms
updateTimer.Tick += (s, e) => {
    TimeSpan elapsed = sw.Elapsed;
    stopwatch.Value = elapsed.ToString(@"mm\:ss\.ff");  // MM:SS.CS format
};

// Start/Stop buttons
Button startBtn = new Button { Text = "Start" };
startBtn.Click += (s, e) => {
    sw.Start();
    updateTimer.Start();
};

Button stopBtn = new Button { Text = "Stop" };
stopBtn.Click += (s, e) => {
    sw.Stop();
    updateTimer.Stop();
};

Button resetBtn = new Button { Text = "Reset" };
resetBtn.Click += (s, e) => {
    sw.Reset();
    stopwatch.Value = "00:00.00";
};

this.Controls.Add(stopwatch);
```

### Scenario 7: Alphanumeric Status Code

```csharp
DigitalGauge codeDisplay = new DigitalGauge();
codeDisplay.Size = new Size(320, 70);
codeDisplay.CharacterType = CharacterType.SixteenSegment;
codeDisplay.CharacterCount = 12;
codeDisplay.SegmentSpacing = 2.0f;
codeDisplay.ForeColor = Color.White;
codeDisplay.BackColor = Color.DarkBlue;
codeDisplay.ShowInvisibleSegments = false;
codeDisplay.Value = "CODE-" + "ABC123";

// Display error codes
void ShowErrorCode(string code)
{
    codeDisplay.ForeColor = Color.Red;
    codeDisplay.Value = "ERR-" + code.PadRight(8);
}

// Display success codes
void ShowSuccessCode(string code)
{
    codeDisplay.ForeColor = Color.Green;
    codeDisplay.Value = "OK--" + code.PadRight(8);
}

this.Controls.Add(codeDisplay);
```

## Troubleshooting

### Issue: Text not visible

**Causes:**
- ForeColor matches BackColor
- Value not set or empty
- CharacterCount too small

**Solution:**
```csharp
gauge.ForeColor = Color.Red;
gauge.BackColor = Color.Black;  // High contrast
gauge.Value = "TEST";
gauge.CharacterCount = Math.Max(4, gauge.Value.Length);
```

### Issue: Characters truncated

**Cause:** CharacterCount smaller than value length

**Solution:**
```csharp
// Ensure character count accommodates value
gauge.CharacterCount = 10;  // Enough for your content
gauge.Value = "HELLO";      // 5 chars fits in 10
```

### Issue: Letters not displaying correctly

**Cause:** Using SevenSegment for alphanumeric text

**Solution:**
```csharp
// Use FourteenSegment or SixteenSegment for letters
gauge.CharacterType = CharacterType.SixteenSegment;
gauge.Value = "HELLO WORLD";  // Now displays correctly
```

### Issue: Display too crowded

**Cause:** Insufficient segment spacing or character count

**Solution:**
```csharp
// Increase spacing
gauge.SegmentSpacing = 4.0f;

// Reduce character count
gauge.CharacterCount = 6;  // Show fewer characters
```

### Issue: Ghost segments too prominent

**Cause:** ShowInvisibleSegments enabled with poor color contrast

**Solution:**
```csharp
// Disable ghost segments
gauge.ShowInvisibleSegments = false;

// Or increase contrast
gauge.ForeColor = Color.Lime;
gauge.BackColor = Color.Black;
```

### Issue: Display appears pixelated or blurry

**Cause:** Gauge size too small for character count

**Solution:**
```csharp
// Increase size proportionally
int charWidth = 30;  // Minimum per character
gauge.Width = gauge.CharacterCount * charWidth;
gauge.Height = 80;  // Adequate height
```

### Issue: Colons or special characters not showing

**Cause:** Character type doesn't support symbol

**Solution:**
```csharp
// Verify character type supports your symbols
gauge.CharacterType = CharacterType.FourteenSegment;  // Better symbol support

// Or adjust display format
gauge.Value = "12 34 56";  // Use spaces instead of colons if needed
```

### Issue: Value updates but display doesn't change

**Cause:** Value assignment issue or display needs refresh

**Solution:**
```csharp
gauge.Value = newValue;
gauge.Refresh();  // Force redraw if needed

// Or verify value is actually changing
Debug.WriteLine($"Setting gauge value to: {newValue}");
gauge.Value = newValue;
```

## Best Practices

1. **Choose appropriate character type** - SevenSegment for numbers, Fourteen/SixteenSegment for alphanumeric
2. **Use ghost segments for clocks** - Provides classic digital clock appearance
3. **Match colors to theme** - Red/Cyan/Green are classic LED colors
4. **Set proper character count** - Allow enough space for your content
5. **Adjust spacing** - Balance readability with compactness
6. **Consider legibility** - Ensure sufficient contrast between fore/back colors
7. **Size appropriately** - ~25-30px width per character minimum
8. **Format consistently** - Use padding/formatting for consistent display (e.g., "00:00:00" vs "0:0:0")
