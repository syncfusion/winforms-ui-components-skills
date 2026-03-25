# Text and Alignment Settings

This guide covers text formatting, shadow effects, and alignment options for the CheckBoxAdv control.

## Table of Contents
- [Text Shadow Effects](#text-shadow-effects)
- [Text Wrapping](#text-wrapping)
- [Text Alignment](#text-alignment)
- [CheckBox Alignment](#checkbox-alignment)
- [Combining Alignments](#combining-alignments)
- [Common Patterns](#common-patterns)

## Text Shadow Effects

The CheckBoxAdv can display text with shadow effects for enhanced visual appearance.

### Shadow Properties

| Property | Type | Description |
|----------|------|-------------|
| TextShadow | bool | Enables or disables text shadow |
| ShadowColor | Color | Color of the text shadow |
| ShadowOffset | Point | Offset distance of shadow from text |

### Enabling Text Shadow

```csharp
// Enable text shadow
checkBoxAdv1.TextShadow = true;
checkBoxAdv1.ShadowColor = Color.Gray;
checkBoxAdv1.ShadowOffset = new Point(2, 2);
```

```vb
' Enable text shadow
checkBoxAdv1.TextShadow = True
checkBoxAdv1.ShadowColor = Color.Gray
checkBoxAdv1.ShadowOffset = New Point(2, 2)
```

### Shadow Offset Explanation

The `ShadowOffset` property uses X and Y coordinates:
- **X value**: Horizontal offset (positive = right, negative = left)
- **Y value**: Vertical offset (positive = down, negative = up)

```csharp
// Shadow to the right and down
checkBoxAdv1.ShadowOffset = new Point(3, 3);

// Shadow to the left and up
checkBoxAdv1.ShadowOffset = new Point(-2, -2);

// Shadow only to the right
checkBoxAdv1.ShadowOffset = new Point(4, 0);

// Shadow only down
checkBoxAdv1.ShadowOffset = new Point(0, 4);
```

### Complete Shadow Example

```csharp
CheckBoxAdv checkBoxWithShadow = new CheckBoxAdv();
checkBoxWithShadow.Text = "Shadowed Text";
checkBoxWithShadow.Location = new Point(20, 20);
checkBoxWithShadow.Size = new Size(200, 30);

// Configure shadow
checkBoxWithShadow.TextShadow = true;
checkBoxWithShadow.ShadowColor = Color.DarkGray;
checkBoxWithShadow.ShadowOffset = new Point(2, 2);
checkBoxWithShadow.ForeColor = Color.Black;

this.Controls.Add(checkBoxWithShadow);
```

### Shadow Color Recommendations

```csharp
// For light backgrounds
checkBoxAdv1.ShadowColor = Color.Gray;
checkBoxAdv1.ShadowColor = Color.DarkGray;
checkBoxAdv1.ShadowColor = Color.FromArgb(64, 64, 64);

// For dark backgrounds
checkBoxAdv1.ShadowColor = Color.White;
checkBoxAdv1.ShadowColor = Color.LightGray;

// For colored backgrounds (complementary colors)
checkBoxAdv1.ForeColor = Color.Blue;
checkBoxAdv1.ShadowColor = Color.Navy;
```

## Text Wrapping

The `WrapText` property controls whether text wraps to multiple lines when it exceeds the control width.

### Enabling Text Wrapping

```csharp
// Enable text wrapping
checkBoxAdv1.WrapText = true;
checkBoxAdv1.Text = "This is a very long text that will wrap to multiple lines";
checkBoxAdv1.Width = 150;
checkBoxAdv1.Height = 60; // Increase height to accommodate wrapped text
```

```vb
' Enable text wrapping
checkBoxAdv1.WrapText = True
checkBoxAdv1.Text = "This is a very long text that will wrap to multiple lines"
checkBoxAdv1.Width = 150
checkBoxAdv1.Height = 60
```

### WrapText Behavior

**When WrapText = true:**
- Text automatically wraps to fit within the control width
- Control height should be adjusted to show all text
- TextContentAlignment may be limited

**When WrapText = false (default):**
- Text displays on a single line
- Excess text is truncated with ellipsis (...)
- Full range of TextContentAlignment options available

### Important Note

When `WrapText = true`, the `TextContentAlignment` property behavior may be affected. For precise text positioning, keep `WrapText = false`.

## Text Alignment

The `TextContentAlignment` property controls the position of the text within the control.

### Alignment Options

The property supports nine alignment positions:

| Value | Description |
|-------|-------------|
| TopLeft | Top-left corner |
| TopCenter | Top-center |
| TopRight | Top-right corner |
| MiddleLeft | Middle-left (default) |
| MiddleCenter | Middle-center |
| MiddleRight | Middle-right |
| BottomLeft | Bottom-left corner |
| BottomCenter | Bottom-center |
| BottomRight | Bottom-right corner |

### Setting Text Alignment

```csharp
// Center the text vertically and horizontally
checkBoxAdv1.TextContentAlignment = ContentAlignment.MiddleCenter;

// Align text to top-left
checkBoxAdv1.TextContentAlignment = ContentAlignment.TopLeft;

// Align text to bottom-right
checkBoxAdv1.TextContentAlignment = ContentAlignment.BottomRight;
```

```vb
' Center the text vertically and horizontally
checkBoxAdv1.TextContentAlignment = ContentAlignment.MiddleCenter

' Align text to top-left
checkBoxAdv1.TextContentAlignment = ContentAlignment.TopLeft

' Align text to bottom-right
checkBoxAdv1.TextContentAlignment = ContentAlignment.BottomRight
```

### Text Alignment Examples

```csharp
// Example 1: Text at top-center
CheckBoxAdv topCenterCheckBox = new CheckBoxAdv();
topCenterCheckBox.Text = "Top Center";
topCenterCheckBox.Size = new Size(200, 60);
topCenterCheckBox.TextContentAlignment = ContentAlignment.TopCenter;
topCenterCheckBox.WrapText = false;

// Example 2: Text at middle-right
CheckBoxAdv middleRightCheckBox = new CheckBoxAdv();
middleRightCheckBox.Text = "Middle Right";
middleRightCheckBox.Size = new Size(200, 40);
middleRightCheckBox.TextContentAlignment = ContentAlignment.MiddleRight;

// Example 3: Text at bottom-left
CheckBoxAdv bottomLeftCheckBox = new CheckBoxAdv();
bottomLeftCheckBox.Text = "Bottom Left";
bottomLeftCheckBox.Size = new Size(200, 60);
bottomLeftCheckBox.TextContentAlignment = ContentAlignment.BottomLeft;
```

## CheckBox Alignment

The `CheckAlign` property controls the position of the checkbox itself within the control.

### CheckBox Positioning

The checkbox can be positioned at any of the nine alignment points, independent of text position:

```csharp
// Checkbox on the right, text on left (default is left)
checkBoxAdv1.CheckAlign = ContentAlignment.MiddleRight;

// Checkbox at top-left
checkBoxAdv1.CheckAlign = ContentAlignment.TopLeft;

// Checkbox at bottom-center
checkBoxAdv1.CheckAlign = ContentAlignment.BottomCenter;
```

```vb
' Checkbox on the right, text on left
checkBoxAdv1.CheckAlign = ContentAlignment.MiddleRight

' Checkbox at top-left
checkBoxAdv1.CheckAlign = ContentAlignment.TopLeft

' Checkbox at bottom-center
checkBoxAdv1.CheckAlign = ContentAlignment.BottomCenter
```

### Common CheckBox Positions

```csharp
// Standard: Checkbox on left, text on right
checkBoxAdv1.CheckAlign = ContentAlignment.MiddleLeft; // Default
checkBoxAdv1.TextContentAlignment = ContentAlignment.MiddleLeft;

// Reversed: Checkbox on right, text on left
checkBoxAdv1.CheckAlign = ContentAlignment.MiddleRight;
checkBoxAdv1.TextContentAlignment = ContentAlignment.MiddleLeft;

// Checkbox above text
checkBoxAdv1.CheckAlign = ContentAlignment.TopCenter;
checkBoxAdv1.TextContentAlignment = ContentAlignment.BottomCenter;

// Checkbox below text
checkBoxAdv1.CheckAlign = ContentAlignment.BottomCenter;
checkBoxAdv1.TextContentAlignment = ContentAlignment.TopCenter;
```

## Combining Alignments

You can create custom layouts by independently positioning the checkbox and text.

### Pattern 1: Checkbox on Right

```csharp
CheckBoxAdv rightAlignedCheckBox = new CheckBoxAdv();
rightAlignedCheckBox.Text = "Text on Left";
rightAlignedCheckBox.Size = new Size(200, 30);

// Checkbox on right side
rightAlignedCheckBox.CheckAlign = ContentAlignment.MiddleRight;
rightAlignedCheckBox.TextContentAlignment = ContentAlignment.MiddleLeft;

this.Controls.Add(rightAlignedCheckBox);
```

### Pattern 2: Vertical Layout

```csharp
CheckBoxAdv verticalCheckBox = new CheckBoxAdv();
verticalCheckBox.Text = "Text Below";
verticalCheckBox.Size = new Size(150, 60);

// Checkbox on top, text below
verticalCheckBox.CheckAlign = ContentAlignment.TopCenter;
verticalCheckBox.TextContentAlignment = ContentAlignment.BottomCenter;

this.Controls.Add(verticalCheckBox);
```

### Pattern 3: Corner Positioning

```csharp
CheckBoxAdv cornerCheckBox = new CheckBoxAdv();
cornerCheckBox.Text = "Opposite Corner";
cornerCheckBox.Size = new Size(200, 60);

// Checkbox in top-left, text in bottom-right
cornerCheckBox.CheckAlign = ContentAlignment.TopLeft;
cornerCheckBox.TextContentAlignment = ContentAlignment.BottomRight;

this.Controls.Add(cornerCheckBox);
```

### Pattern 4: Centered Layout

```csharp
CheckBoxAdv centeredCheckBox = new CheckBoxAdv();
centeredCheckBox.Text = "Centered";
centeredCheckBox.Size = new Size(200, 60);

// Both checkbox and text centered
centeredCheckBox.CheckAlign = ContentAlignment.MiddleCenter;
centeredCheckBox.TextContentAlignment = ContentAlignment.MiddleCenter;
// Note: Text will overlap checkbox in this configuration

this.Controls.Add(centeredCheckBox);
```

## Common Patterns

### Pattern 1: Multi-line Checkbox with Wrapped Text

```csharp
CheckBoxAdv multiLineCheckBox = new CheckBoxAdv();
multiLineCheckBox.Text = "I agree to the terms and conditions of this agreement and understand all implications";
multiLineCheckBox.Location = new Point(20, 20);
multiLineCheckBox.Size = new Size(300, 60);
multiLineCheckBox.WrapText = true;
multiLineCheckBox.CheckAlign = ContentAlignment.TopLeft;
```

### Pattern 2: Enhanced Visual Checkbox

```csharp
CheckBoxAdv enhancedCheckBox = new CheckBoxAdv();
enhancedCheckBox.Text = "Premium Feature";
enhancedCheckBox.Size = new Size(250, 40);
enhancedCheckBox.Font = new Font("Arial", 10, FontStyle.Bold);
enhancedCheckBox.ForeColor = Color.DarkBlue;

// Add shadow effect
enhancedCheckBox.TextShadow = true;
enhancedCheckBox.ShadowColor = Color.LightBlue;
enhancedCheckBox.ShadowOffset = new Point(2, 2);

// Center alignment
enhancedCheckBox.TextContentAlignment = ContentAlignment.MiddleCenter;
enhancedCheckBox.CheckAlign = ContentAlignment.MiddleLeft;
```

### Pattern 3: Right-to-Left Layout

```csharp
CheckBoxAdv rtlCheckBox = new CheckBoxAdv();
rtlCheckBox.Text = "Right-Aligned Option";
rtlCheckBox.Size = new Size(200, 30);
rtlCheckBox.CheckAlign = ContentAlignment.MiddleRight;
rtlCheckBox.TextContentAlignment = ContentAlignment.MiddleRight;
rtlCheckBox.RightToLeft = RightToLeft.Yes;
```

### Pattern 4: Compact Checkbox with Top Alignment

```csharp
CheckBoxAdv compactCheckBox = new CheckBoxAdv();
compactCheckBox.Text = "Compact";
compactCheckBox.Size = new Size(120, 25);
compactCheckBox.TextContentAlignment = ContentAlignment.MiddleLeft;
compactCheckBox.CheckAlign = ContentAlignment.TopLeft;
compactCheckBox.Font = new Font("Arial", 8);
```

## Best Practices

### Text Alignment
- Use **MiddleLeft** (default) for standard checkbox layout
- Use **MiddleCenter** when checkbox is in a centered container
- Keep **WrapText = false** when using complex alignments

### Shadow Effects
- Use subtle offsets (1-3 pixels) for professional appearance
- Match shadow color to background for depth effect
- Avoid shadows on small fonts (<8pt)

### CheckBox Positioning
- **MiddleLeft**: Standard Windows convention
- **MiddleRight**: Use for right-aligned forms
- **TopLeft**: Use with multi-line wrapped text
- **TopCenter/BottomCenter**: Use for icon-style layouts
