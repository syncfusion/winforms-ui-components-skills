# Foreground and Text Settings

Complete guide to configuring text appearance, alignment, font styling, and foreground color in GradientLabel.

## Overview

GradientLabel provides rich text customization through:
- **Text**: Label content
- **ForeColor**: Text color
- **Font**: Typography settings
- **TextAlign**: Content alignment
- **DrawActiveWhenDisabled**: Appearance when disabled

---

## Text Property

Sets the displayed text content.

**Property Type:** `string`  
**Default Value:** Empty string

### Basic Text

**C# Example:**
```csharp
gradientLabel.Text = "Welcome to Application";
```

**VB.NET Example:**
```vb
gradientLabel.Text = "Welcome to Application"
```

---

### Multi-Line Text

Use `\n` for line breaks.

**C# Example:**
```csharp
gradientLabel.Text = "Line 1\nLine 2\nLine 3";

// Or with verbatim string
gradientLabel.Text = @"First Line
Second Line
Third Line";
```

**VB.NET Example:**
```vb
gradientLabel.Text = "Line 1" & vbCrLf & "Line 2" & vbCrLf & "Line 3"

' Or
gradientLabel.Text = "First Line" & Environment.NewLine & "Second Line"
```

---

### Dynamic Text

**C# Example:**
```csharp
// From variable
string userName = "John Doe";
gradientLabel.Text = $"Welcome, {userName}!";

// From calculation
int itemCount = 42;
gradientLabel.Text = $"Total Items: {itemCount}";

// Date/Time
gradientLabel.Text = DateTime.Now.ToString("MMM dd, yyyy - hh:mm tt");
```

**VB.NET Example:**
```vb
' From variable
Dim userName As String = "John Doe"
gradientLabel.Text = $"Welcome, {userName}!"

' From calculation
Dim itemCount As Integer = 42
gradientLabel.Text = $"Total Items: {itemCount}"

' Date/Time
gradientLabel.Text = DateTime.Now.ToString("MMM dd, yyyy - hh:mm tt")
```

---

## ForeColor Property

Sets the text color.

**Property Type:** `Color`  
**Default Value:** `Control.DefaultForeColor`

### Standard Colors

**C# Example:**
```csharp
gradientLabel.ForeColor = Color.White;
gradientLabel.ForeColor = Color.Black;
gradientLabel.ForeColor = Color.DarkBlue;
```

**VB.NET Example:**
```vb
gradientLabel.ForeColor = Color.White
gradientLabel.ForeColor = Color.Black
gradientLabel.ForeColor = Color.DarkBlue
```

---

### Custom RGB Colors

**C# Example:**
```csharp
// RGB format
gradientLabel.ForeColor = Color.FromArgb(255, 100, 50);

// ARGB with transparency
gradientLabel.ForeColor = Color.FromArgb(200, 255, 100, 50);

// Hex color
gradientLabel.ForeColor = ColorTranslator.FromHtml("#FF6347");  // Tomato
```

**VB.NET Example:**
```vb
' RGB format
gradientLabel.ForeColor = Color.FromArgb(255, 100, 50)

' ARGB with transparency
gradientLabel.ForeColor = Color.FromArgb(200, 255, 100, 50)

' Hex color
gradientLabel.ForeColor = ColorTranslator.FromHtml("#FF6347")  ' Tomato
```

---

### Contrast with Background

**C# Example:**
```csharp
// Dark background → Light text
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.DarkBlue,
    Color.Blue
);
gradientLabel.ForeColor = Color.White;

// Light background → Dark text
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.LightGray,
    Color.White
);
gradientLabel.ForeColor = Color.Black;
```

**VB.NET Example:**
```vb
' Dark background → Light text
gradientLabel.BackgroundColor = New BrushInfo( _
    GradientStyle.Vertical, _
    Color.DarkBlue, _
    Color.Blue _
)
gradientLabel.ForeColor = Color.White

' Light background → Dark text
gradientLabel.BackgroundColor = New BrushInfo( _
    GradientStyle.Horizontal, _
    Color.LightGray, _
    Color.White _
)
gradientLabel.ForeColor = Color.Black
```

---

## Font Property

Configures typography for the text.

**Property Type:** `Font`  
**Default Value:** Control's parent font

### Basic Font Configuration

**C# Example:**
```csharp
// Family and size
gradientLabel.Font = new Font("Arial", 12);

// With style
gradientLabel.Font = new Font("Segoe UI", 14, FontStyle.Bold);

// Multiple styles
gradientLabel.Font = new Font("Verdana", 10, FontStyle.Bold | FontStyle.Italic);
```

**VB.NET Example:**
```vb
' Family and size
gradientLabel.Font = New Font("Arial", 12)

' With style
gradientLabel.Font = New Font("Segoe UI", 14, FontStyle.Bold)

' Multiple styles
gradientLabel.Font = New Font("Verdana", 10, FontStyle.Bold Or FontStyle.Italic)
```

---

### Font Styles

| Style | Description |
|-------|-------------|
| **Regular** | Normal weight |
| **Bold** | Heavy weight |
| **Italic** | Slanted |
| **Underline** | Underlined text |
| **Strikeout** | Line through text |

**C# Example:**
```csharp
// Bold
gradientLabel.Font = new Font("Arial", 12, FontStyle.Bold);

// Italic
gradientLabel.Font = new Font("Arial", 12, FontStyle.Italic);

// Bold + Italic
gradientLabel.Font = new Font("Arial", 12, FontStyle.Bold | FontStyle.Italic);

// Underline
gradientLabel.Font = new Font("Arial", 12, FontStyle.Underline);
```

---

### Font Size Guidelines

| Size (pt) | Usage |
|-----------|-------|
| **8-10** | Small labels, captions |
| **11-12** | Body text, standard labels |
| **14-16** | Subheadings |
| **18-24** | Headings |
| **28+** | Large titles |

---

## TextAlign Property

Controls horizontal and vertical text alignment.

**Property Type:** `ContentAlignment` (enum)  
**Default Value:** `ContentAlignment.TopLeft`

### Alignment Options

**Horizontal:** Left, Center, Right  
**Vertical:** Top, Middle, Bottom

| Value | Description |
|-------|-------------|
| **TopLeft** | Top-left corner |
| **TopCenter** | Top edge, centered |
| **TopRight** | Top-right corner |
| **MiddleLeft** | Left edge, vertically centered |
| **MiddleCenter** | Fully centered |
| **MiddleRight** | Right edge, vertically centered |
| **BottomLeft** | Bottom-left corner |
| **BottomCenter** | Bottom edge, centered |
| **BottomRight** | Bottom-right corner |

---

### Common Alignments

**C# Example:**
```csharp
// Centered (most common for headers)
gradientLabel.TextAlign = ContentAlignment.MiddleCenter;

// Left-aligned
gradientLabel.TextAlign = ContentAlignment.MiddleLeft;

// Right-aligned (for numbers)
gradientLabel.TextAlign = ContentAlignment.MiddleRight;

// Top-left (default)
gradientLabel.TextAlign = ContentAlignment.TopLeft;
```

**VB.NET Example:**
```vb
' Centered (most common for headers)
gradientLabel.TextAlign = ContentAlignment.MiddleCenter

' Left-aligned
gradientLabel.TextAlign = ContentAlignment.MiddleLeft

' Right-aligned (for numbers)
gradientLabel.TextAlign = ContentAlignment.MiddleRight

' Top-left (default)
gradientLabel.TextAlign = ContentAlignment.TopLeft
```

---

## DrawActiveWhenDisabled Property

Controls whether the label appears grayed out when disabled.

**Property Type:** `bool`  
**Default Value:** `false`

### Standard Disabled Appearance (Default)

**C# Example:**
```csharp
gradientLabel.DrawActiveWhenDisabled = false;  // Gray when disabled
gradientLabel.Enabled = false;
```

**VB.NET Example:**
```vb
gradientLabel.DrawActiveWhenDisabled = False  ' Gray when disabled
gradientLabel.Enabled = False
```

**Result:** Text appears grayed/dimmed when `Enabled = false`

---

### Active Disabled Appearance

**C# Example:**
```csharp
gradientLabel.DrawActiveWhenDisabled = true;  // Keep colors when disabled
gradientLabel.Enabled = false;
```

**VB.NET Example:**
```vb
gradientLabel.DrawActiveWhenDisabled = True  ' Keep colors when disabled
gradientLabel.Enabled = False
```

**Result:** Text keeps full color even when `Enabled = false`

**Use Case:** Display-only labels that should remain visually prominent but not interactive.

---

## Complete Examples

### Example 1: Header Label

```csharp
GradientLabel headerLabel = new GradientLabel
{
    Size = new Size(400, 50),
    Text = "Application Dashboard",
    Font = new Font("Segoe UI", 18, FontStyle.Bold),
    ForeColor = Color.White,
    TextAlign = ContentAlignment.MiddleCenter
};

headerLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.DarkBlue,
    Color.DodgerBlue
);
```

---

### Example 2: Status Label

```csharp
GradientLabel statusLabel = new GradientLabel
{
    Size = new Size(120, 30),
    Text = "Active",
    Font = new Font("Arial", 10, FontStyle.Bold),
    ForeColor = Color.White,
    TextAlign = ContentAlignment.MiddleCenter
};

statusLabel.BackgroundColor = new BrushInfo(
    BrushStyle.Solid,
    Color.Green,
    Color.White
);
```

---

### Example 3: Multi-Line Information Panel

```csharp
GradientLabel infoLabel = new GradientLabel
{
    Size = new Size(300, 80),
    Text = "System Status\n\nAll services running normally\nLast check: " + DateTime.Now.ToShortTimeString(),
    Font = new Font("Segoe UI", 9, FontStyle.Regular),
    ForeColor = Color.Black,
    TextAlign = ContentAlignment.TopLeft
};

infoLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.WhiteSmoke,
    Color.LightGray
);

// Add padding for better readability
infoLabel.Padding = new Padding(10);
```

---

### Example 4: Right-Aligned Number Display

```csharp
GradientLabel countLabel = new GradientLabel
{
    Size = new Size(150, 40),
    Text = "1,234",
    Font = new Font("Consolas", 14, FontStyle.Bold),  // Monospace for numbers
    ForeColor = Color.DarkBlue,
    TextAlign = ContentAlignment.MiddleRight
};

countLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.LightBlue,
    Color.White
);

// Padding on right for spacing
countLabel.Padding = new Padding(0, 0, 10, 0);
```

---

### Example 5: Disabled Display Label

```csharp
GradientLabel displayLabel = new GradientLabel
{
    Size = new Size(200, 35),
    Text = "Read-Only Value",
    Font = new Font("Arial", 10, FontStyle.Regular),
    ForeColor = Color.DarkSlateGray,
    TextAlign = ContentAlignment.MiddleLeft,
    Enabled = false,
    DrawActiveWhenDisabled = true  // Keep full color when disabled
};

displayLabel.BackgroundColor = new BrushInfo(
    BrushStyle.Solid,
    Color.Gainsboro,
    Color.White
);
```

---

## Text Styling Best Practices

### 1. Font Selection

```csharp
// Headers: Bold sans-serif
headerLabel.Font = new Font("Segoe UI", 16, FontStyle.Bold);

// Body text: Regular sans-serif
bodyLabel.Font = new Font("Arial", 10, FontStyle.Regular);

// Numbers/code: Monospace
dataLabel.Font = new Font("Consolas", 11, FontStyle.Regular);
```

### 2. Color Contrast

**WCAG Guidelines:**
- Normal text: 4.5:1 contrast ratio minimum
- Large text (18pt+): 3:1 contrast ratio minimum

```csharp
// Good contrast
gradientLabel.BackgroundColor = new BrushInfo(BrushStyle.Solid, Color.Navy, Color.White);
gradientLabel.ForeColor = Color.White;  // High contrast

// Poor contrast (avoid)
gradientLabel.BackgroundColor = new BrushInfo(BrushStyle.Solid, Color.LightGray, Color.White);
gradientLabel.ForeColor = Color.Gray;  // Low contrast
```

### 3. Text Alignment

```csharp
// Headers: Center
headerLabel.TextAlign = ContentAlignment.MiddleCenter;

// Paragraphs: Left
paragraphLabel.TextAlign = ContentAlignment.TopLeft;

// Numbers/data: Right
numberLabel.TextAlign = ContentAlignment.MiddleRight;

// Status badges: Center
statusLabel.TextAlign = ContentAlignment.MiddleCenter;
```

### 4. Font Sizing Hierarchy

```csharp
// Large title
titleLabel.Font = new Font("Segoe UI", 24, FontStyle.Bold);

// Section header
sectionLabel.Font = new Font("Segoe UI", 16, FontStyle.Bold);

// Subsection
subsectionLabel.Font = new Font("Segoe UI", 12, FontStyle.Bold);

// Body text
bodyLabel.Font = new Font("Segoe UI", 10, FontStyle.Regular);

// Caption
captionLabel.Font = new Font("Segoe UI", 8, FontStyle.Regular);
```

### 5. Multi-Line Text with Padding

```csharp
// Add padding for multi-line readability
multiLineLabel.Padding = new Padding(10, 8, 10, 8);
multiLineLabel.TextAlign = ContentAlignment.TopLeft;
multiLineLabel.Text = "Line 1\nLine 2\nLine 3";
```

---

## Common Text Patterns

### Pattern 1: Centered Header

```csharp
gradientLabel.TextAlign = ContentAlignment.MiddleCenter;
gradientLabel.Font = new Font("Segoe UI", 16, FontStyle.Bold);
```

### Pattern 2: Left-Aligned Body

```csharp
gradientLabel.TextAlign = ContentAlignment.TopLeft;
gradientLabel.Font = new Font("Arial", 10, FontStyle.Regular);
gradientLabel.Padding = new Padding(10);
```

### Pattern 3: Right-Aligned Numbers

```csharp
gradientLabel.TextAlign = ContentAlignment.MiddleRight;
gradientLabel.Font = new Font("Consolas", 12, FontStyle.Regular);
```

### Pattern 4: Status Badge

```csharp
gradientLabel.TextAlign = ContentAlignment.MiddleCenter;
gradientLabel.Font = new Font("Arial", 9, FontStyle.Bold);
gradientLabel.Text = gradientLabel.Text.ToUpper();
```

---

## Troubleshooting

### Text Not Visible

**Check:**
1. ForeColor contrasts with background
2. Text property is not empty
3. Font size is appropriate for control size
4. Control size is adequate for text content

### Text Truncated

**Solutions:**
```csharp
// Increase control size
gradientLabel.Size = new Size(300, 60);

// Reduce font size
gradientLabel.Font = new Font("Arial", 9);

// Use AutoSize (if available)
gradientLabel.AutoSize = true;
```

### Multi-Line Text on Single Line

**Solution:** Ensure adequate height and use proper line breaks.

```csharp
gradientLabel.Size = new Size(200, 60);  // Height for 3 lines
gradientLabel.Text = "Line 1\nLine 2\nLine 3";
gradientLabel.TextAlign = ContentAlignment.TopLeft;
```

### Font Not Applied

**Solution:** Verify font exists on system.

```csharp
// Check font availability
FontFamily fontFamily = new FontFamily("YourFontName");
if (fontFamily != null)
{
    gradientLabel.Font = new Font(fontFamily, 12);
}
else
{
    // Fallback
    gradientLabel.Font = new Font("Arial", 12);
}
```

---

## Related Topics

- **Background Styling**: Gradient backgrounds → [background-styling.md](background-styling.md)
- **Border Configuration**: Border appearance → [border-configuration.md](border-configuration.md)
- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)
