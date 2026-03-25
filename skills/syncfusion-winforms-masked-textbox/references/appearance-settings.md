# Appearance Settings

Configure the visual look and feel of the MaskedEditBox control including borders, colors, fonts, and display options.

## Border Settings

### Border3DStyle Property

Controls the 3D appearance and visual depth of the control's border.

**Available Styles:**

| Style | Appearance | Use Case |
|-------|-----------|----------|
| `Flat` | No 3D effect, simple line | Modern, flat UI |
| `Raised` | Outer edges elevated (embossed) | Classic WinForms style |
| `Sunken` | Outer edges recessed (inset) | Standard input field look |
| `RaisedOuter` | 3D outer raised effect | Grouped control appearance |
| `SunkenOuter` | 3D outer sunken effect | Form section separation |
| `RaisedInner` | 3D inner raised effect | Advanced styling |
| `SunkenInner` | 3D inner sunken effect | Advanced styling |
| `Bump` | Raised bumpy texture | Textured appearance |
| `Etched` | Etched line effect | Vintage appearance |

### Setting Border Style

```csharp
maskedEditBox.Border3DStyle = Border3DStyle.Sunken;
```

### Examples

**Standard Input Field (Recommended):**
```csharp
maskedEditBox.Border3DStyle = Border3DStyle.Sunken;
maskedEditBox.BackColor = Color.White;
// Looks like standard textbox
```

**Modern Flat Design:**
```csharp
maskedEditBox.Border3DStyle = Border3DStyle.Flat;
maskedEditBox.BackColor = Color.LightGray;
// Minimalist appearance
```

**Raised/Highlighted:**
```csharp
maskedEditBox.Border3DStyle = Border3DStyle.Raised;
// Emphasizes input field
```

## Color Properties

### ForeColor (Text Color)

Sets the color of the text displayed in the control.

```csharp
// Black text (default)
maskedEditBox.ForeColor = Color.Black;

// Blue text
maskedEditBox.ForeColor = Color.Blue;

// RGB custom color
maskedEditBox.ForeColor = Color.FromArgb(64, 64, 64);  // Dark gray
```

### BackColor (Background Color)

Sets the background color of the input area.

```csharp
// White background (default)
maskedEditBox.BackColor = Color.White;

// Light gray background
maskedEditBox.BackColor = Color.LightGray;

// Custom light color
maskedEditBox.BackColor = Color.FromArgb(245, 245, 245);  // Off-white
```

### DisabledForeColor

Sets text color when control is disabled.

```csharp
maskedEditBox.DisabledForeColor = Color.Gray;
```

### DisabledBackColor

Sets background color when control is disabled.

```csharp
maskedEditBox.DisabledBackColor = Color.LightGray;
```

### Example: Themed Input Field

```csharp
private void ApplyDarkTheme()
{
    maskedEditBox.BackColor = Color.FromArgb(30, 30, 30);      // Dark background
    maskedEditBox.ForeColor = Color.FromArgb(200, 200, 200);   // Light text
    maskedEditBox.Border3DStyle = Border3DStyle.Flat;
}

private void ApplyLightTheme()
{
    maskedEditBox.BackColor = Color.White;
    maskedEditBox.ForeColor = Color.Black;
    maskedEditBox.Border3DStyle = Border3DStyle.Sunken;
}
```

## Font Properties

### Font Property

Controls the typeface, size, style of text in the control.

```csharp
// Set font by name, size, and style
maskedEditBox.Font = new Font("Arial", 10, FontStyle.Regular);

// Bold font
maskedEditBox.Font = new Font("Arial", 10, FontStyle.Bold);

// Multiple styles
maskedEditBox.Font = new Font("Arial", 10, FontStyle.Bold | FontStyle.Italic);

// Monospace font (good for codes)
maskedEditBox.Font = new Font("Courier New", 10);
```

### Font Size Guidelines

| Size | Usage |
|------|-------|
| 8-9 | Small labels, secondary info |
| 10-11 | Standard input fields |
| 12-14 | Primary input, emphasis |
| 16+ | Headings, large displays |

### Common Font Choices

```csharp
// Professional input field
maskedEditBox.Font = new Font("Segoe UI", 10);

// Code/technical data (IP, MAC address)
maskedEditBox.Font = new Font("Courier New", 10);

// Large entry field
maskedEditBox.Font = new Font("Arial", 12);

// Minimal modern
maskedEditBox.Font = new Font("Tahoma", 10);
```

## Display Options

### ReadOnly Property

Prevents user input while allowing programmatic changes and text selection.

```csharp
// Allow user editing (default)
maskedEditBox.ReadOnly = false;

// Prevent user editing
maskedEditBox.ReadOnly = true;
// User can select/copy but not modify
```

**Use Cases:**
- Display-only fields (confirmation view)
- Status information that shouldn't be edited
- Reference data that's updated programmatically

```csharp
// Confirm phone number display
maskedEditBox.Mask = "(###) ###-####";
maskedEditBox.Value = "5551234567";
maskedEditBox.ReadOnly = true;
// User can see and copy, but not change
```

### Enabled Property

Disables the control entirely (grayed out, no interaction).

```csharp
// Normal state (enabled)
maskedEditBox.Enabled = true;

// Disabled state
maskedEditBox.Enabled = false;
// Control is grayed out, no interaction allowed
```

**Use Cases:**
- Conditional fields (disabled until prerequisite filled)
- Locked data during processing
- Permission-based access control

```csharp
// Disable extension field if no extension selected
if (hasExtension)
{
    extensionField.Enabled = true;
}
else
{
    extensionField.Enabled = false;
}
```

### Visible Property

Controls whether the control is displayed or hidden.

```csharp
// Show control
maskedEditBox.Visible = true;

// Hide control
maskedEditBox.Visible = false;
```

**Use Cases:**
- Conditional display based on form state
- Multi-step forms
- Alternative input methods

## Text Alignment

### TextAlign Property

Controls horizontal alignment of text within the control.

```csharp
// Left-aligned (default)
maskedEditBox.TextAlign = HorizontalAlignment.Left;

// Center-aligned
maskedEditBox.TextAlign = HorizontalAlignment.Center;

// Right-aligned (common for numbers)
maskedEditBox.TextAlign = HorizontalAlignment.Right;
```

### Alignment Guidelines

| Alignment | Best For |
|-----------|----------|
| Left | Text, phone numbers, names |
| Center | Codes, short data, emphasis |
| Right | Numbers, currency, amounts |

### Examples

```csharp
// Phone number - left aligned (default)
phoneField.TextAlign = HorizontalAlignment.Left;

// Account number - center for emphasis
accountField.TextAlign = HorizontalAlignment.Center;

// Amount - right aligned (standard for numbers)
amountField.TextAlign = HorizontalAlignment.Right;
```

## Size and Layout

### Size Property

Sets width and height in pixels.

```csharp
// Small input
maskedEditBox.Size = new Size(150, 25);

// Standard width
maskedEditBox.Size = new Size(200, 25);

// Wide input
maskedEditBox.Size = new Size(300, 25);
```

### Separate Width and Height

```csharp
maskedEditBox.Width = 200;
maskedEditBox.Height = 25;
```

### Location Property

Sets position on the form.

```csharp
maskedEditBox.Location = new Point(10, 10);  // 10px from left, 10px from top
```

### AutoSize Property

Automatically adjusts size based on content.

```csharp
maskedEditBox.AutoSize = true;
```

## Complete Styling Example

```csharp
private void ConfigurePhoneInput()
{
    // Create and configure MaskedEditBox
    MaskedEditBox phoneField = new MaskedEditBox();
    
    // Mask and data
    phoneField.Mask = "(###) ###-####";
    phoneField.InputMode = InputModes.IgnoreSeparators;
    
    // Appearance
    phoneField.BackColor = Color.White;
    phoneField.ForeColor = Color.Black;
    phoneField.Font = new Font("Segoe UI", 10);
    phoneField.Border3DStyle = Border3DStyle.Sunken;
    phoneField.TextAlign = HorizontalAlignment.Left;
    
    // Size and position
    phoneField.Size = new Size(200, 25);
    phoneField.Location = new Point(10, 10);
    
    // Add to form
    this.Controls.Add(phoneField);
}

private void ConfigureReadOnlyDisplay()
{
    MaskedEditBox displayField = new MaskedEditBox();
    
    // Mask
    displayField.Mask = "(###) ###-####";
    
    // Display only
    displayField.ReadOnly = true;
    
    // Appearance - subtle styling
    displayField.BackColor = Color.LightGray;
    displayField.ForeColor = Color.DarkGray;
    displayField.Font = new Font("Arial", 9);
    displayField.Border3DStyle = Border3DStyle.Flat;
    
    // Data
    displayField.Value = "5551234567";
    
    this.Controls.Add(displayField);
}
```

## Responsive Styling

### Scaling Based on Form Size

```csharp
private void Form_SizeChanged(object sender, EventArgs e)
{
    // Make input field responsive
    int formWidth = this.ClientSize.Width;
    maskedEditBox.Width = Math.Max(formWidth - 40, 150);  // Min 150px
}
```

### Adaptive Font Sizes

```csharp
private void SetResponsiveFont()
{
    if (this.Width < 400)
    {
        maskedEditBox.Font = new Font("Arial", 9);
    }
    else if (this.Width < 800)
    {
        maskedEditBox.Font = new Font("Arial", 10);
    }
    else
    {
        maskedEditBox.Font = new Font("Arial", 11);
    }
}
```

## Best Practices

1. **Consistency** - Use matching styles across all MaskedEditBox controls in your application
2. **Contrast** - Ensure text color has sufficient contrast with background for readability
3. **Standard sizes** - Use 10-11pt fonts for standard inputs, 12pt for emphasis
4. **Borders** - Use Sunken for input, Flat for modern designs
5. **Readability** - Don't use light text on light backgrounds or dark on dark
6. **Disabled state** - Always configure DisabledForeColor and DisabledBackColor for clarity
7. **Test readability** - Verify appearance works for colorblind users (avoid red/green only)
