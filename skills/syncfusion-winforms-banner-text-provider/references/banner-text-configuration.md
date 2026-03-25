# Banner Text Configuration

## Table of Contents
- [Text and Visibility](#text-and-visibility)
- [Color Customization](#color-customization)
- [Font Customization](#font-customization)
- [Display Modes](#display-modes)
- [Configuration Examples](#configuration-examples)
- [Dynamic Changes](#dynamic-changes)

## Text and Visibility

### Setting Banner Text
The `Text` property holds the watermark content displayed to users:

```csharp
var banner = new BannerTextInfo()
{
    Text = "Enter your name...",
    Visible = true
};

bannerTextProvider1.SetBannerText(nameTextBox, banner);
```

### Visibility Control
Toggle banner text visibility without removing the configuration:

```csharp
var banner = new BannerTextInfo()
{
    Text = "This text will be hidden",
    Visible = false  // Banner text not displayed
};

bannerTextProvider1.SetBannerText(textBox, banner);
```

**Use Cases for Visible = false:**
- Temporarily hide banner text during validation
- Conditionally display hints based on user state
- Debug scenarios without removing configuration

### Multi-line Text
While banner text is typically single-line, you can use escape sequences:

```csharp
var banner = new BannerTextInfo()
{
    Text = "Enter information\r\n(Required field)",
    Visible = true
};

bannerTextProvider1.SetBannerText(textBox, banner);
```

**Note:** Multi-line support depends on the editor control type

## Color Customization

### Setting Text Color
Use the `Color` property to change banner text appearance:

```csharp
var banner = new BannerTextInfo()
{
    Text = "Enter email...",
    Visible = true,
    Color = Color.Gray
};

bannerTextProvider1.SetBannerText(emailTextBox, banner);
```

### Common Color Patterns

**Light Gray (subtle hint):**
```csharp
Color = Color.LightGray
```

**Theme-matched colors:**
```csharp
// Use system colors for accessibility
Color = SystemColors.GrayText
```

**Custom RGB:**
```csharp
Color = Color.FromArgb(150, 150, 150)  // RGB: (150, 150, 150)
```

**Semantic Colors:**
```csharp
// Information fields
Color = Color.FromArgb(70, 130, 180)  // Steel blue

// Validation hints
Color = Color.FromArgb(220, 20, 60)   // Crimson for warnings

// Success hints
Color = Color.FromArgb(34, 139, 34)   // Forest green
```

### Color Visibility Best Practices

✓ **Contrast:** Ensure banner text color contrasts with control background
✓ **Focus state:** Choose colors that remain visible but don't blend with typed text
✓ **Accessibility:** Avoid relying on color alone; combine with italic/different font
✓ **Testing:** Test on different themes and backgrounds

**Example: Accessible Banner Configuration**
```csharp
var banner = new BannerTextInfo()
{
    Text = "Required field",
    Visible = true,
    Color = SystemColors.GrayText,  // High contrast
    Font = new Font("Verdana", 9, FontStyle.Italic)  // Italic adds distinction
};

bannerTextProvider1.SetBannerText(textBox, banner);
```

## Font Customization

### Basic Font Customization
Use the `Font` property to control text appearance:

```csharp
var banner = new BannerTextInfo()
{
    Text = "Enter text...",
    Visible = true,
    Font = new Font("Verdana", 9, FontStyle.Italic)
};

bannerTextProvider1.SetBannerText(textBox, banner);
```

### Font Properties

| Parameter | Purpose | Example |
|-----------|---------|---------|
| **Font Family** | Typeface name | "Verdana", "Arial", "Consolas" |
| **Size** | Point size | 9, 10, 11 |
| **Style** | Font styling | Italic, Bold, Underline |

### Font Style Combinations

**Italic (gentle hint):**
```csharp
Font = new Font("Segoe UI", 9, FontStyle.Italic)
```

**Bold (prominent hint):**
```csharp
Font = new Font("Arial", 9, FontStyle.Bold)
```

**Italic + Bold:**
```csharp
Font = new Font("Verdana", 8, FontStyle.Italic | FontStyle.Bold)
```

**Underline (for special cases):**
```csharp
Font = new Font("Arial", 9, FontStyle.Underline)
```

### Recommended Font Combinations

**Subtle, Professional:**
```csharp
Font = new Font("Segoe UI", 9, FontStyle.Italic)
Color = SystemColors.GrayText
```

**Clear, Attention-Getting:**
```csharp
Font = new Font("Arial", 10, FontStyle.Bold)
Color = Color.DarkGray
```

**Minimal, Unobtrusive:**
```csharp
Font = new Font("Tahoma", 8, FontStyle.Regular)
Color = Color.LightGray
```

### Dynamic Font Selection

```csharp
// Use control's font as base, make italic
Font controlFont = textBox.Font;
var bannerFont = new Font(
    controlFont.FontFamily,
    controlFont.Size,
    FontStyle.Italic
);

var banner = new BannerTextInfo()
{
    Text = "Hint text",
    Visible = true,
    Font = bannerFont
};

bannerTextProvider1.SetBannerText(textBox, banner);
```

## Display Modes

### FocusMode
Banner text **disappears when the control receives focus**:

```csharp
var banner = new BannerTextInfo()
{
    Text = "Click here to edit",
    Visible = true,
    Mode = BannerTextMode.FocusMode
};

bannerTextProvider1.SetBannerText(textBox, banner);
```

**Behavior:**
- Banner displayed when control is not focused
- Banner hidden as soon as user clicks/tabs to control
- Reappears when control loses focus (if empty)
- Good for temporary guidance/hints

**Use When:**
- Providing temporary instructional text
- Hints that shouldn't interfere with editing
- Quick visual guides for users

### EditMode
Banner text **disappears only when the control contains text**:

```csharp
var banner = new BannerTextInfo()
{
    Text = "Enter your email address",
    Visible = true,
    Mode = BannerTextMode.EditMode
};

bannerTextProvider1.SetBannerText(emailTextBox, banner);
```

**Behavior:**
- Banner displayed when control is empty
- Banner visible even when control is focused
- Banner hidden only when user types (control has text)
- Reappears when user clears the field
- Good for persistent watermarks/placeholders

**Use When:**
- Providing persistent field labels/hints
- Clear differentiation between placeholder and content
- Important information that shouldn't be missed
- Form fields that need persistent guidance

### Mode Comparison

| Aspect | FocusMode | EditMode |
|--------|-----------|----------|
| **Disappears** | On focus | On text entry |
| **Reappears** | On blur (if empty) | When field cleared |
| **Best For** | Quick hints | Persistent placeholders |
| **Visible During Edit** | No | Yes (until typed) |

**Decision Guide:**
```
Choose FocusMode if:
- Text is brief, temporary instruction
- You want minimal on-screen clutter while typing
- Hint is for user orientation only

Choose EditMode if:
- Text identifies the field (label-like)
- User might forget field purpose while typing
- Text must remain visible until content entered
- Form has complex layout needing persistent hints
```

## Configuration Examples

### Example 1: Search Field (FocusMode)
```csharp
var searchBanner = new BannerTextInfo()
{
    Text = "Search by name, email, or ID...",
    Visible = true,
    Mode = BannerTextMode.FocusMode,
    Color = SystemColors.GrayText,
    Font = new Font("Arial", 9, FontStyle.Italic)
};

bannerTextProvider1.SetBannerText(searchTextBox, searchBanner);
```

### Example 2: Email Field (EditMode)
```csharp
var emailBanner = new BannerTextInfo()
{
    Text = "user@example.com",
    Visible = true,
    Mode = BannerTextMode.EditMode,
    Color = Color.FromArgb(100, 100, 100),
    Font = new Font("Segoe UI", 9, FontStyle.Italic)
};

bannerTextProvider1.SetBannerText(emailTextBox, emailBanner);
```

### Example 3: Comments Field (EditMode)
```csharp
var commentsBanner = new BannerTextInfo()
{
    Text = "Enter your comments here (optional)",
    Visible = true,
    Mode = BannerTextMode.EditMode,
    Color = Color.DarkGray,
    Font = new Font("Verdana", 9, FontStyle.Italic)
};

bannerTextProvider1.SetBannerText(commentsTextBox, commentsBanner);
```

## Dynamic Changes

### Update Banner Text at Runtime
```csharp
var updatedBanner = new BannerTextInfo()
{
    Text = "New banner text",
    Visible = true,
    Color = Color.Green,
    Mode = BannerTextMode.EditMode
};

bannerTextProvider1.SetBannerText(textBox, updatedBanner);
```

### Toggle Visibility
```csharp
// Get current banner configuration
var banner = new BannerTextInfo()
{
    Text = "Current text",
    Visible = false  // Toggle visibility
};

bannerTextProvider1.SetBannerText(textBox, banner);
```

### Change Mode Based on Validation
```csharp
private void TextBox_Validating(object sender, CancelEventArgs e)
{
    var textBox = sender as TextBoxExt;
    var mode = textBox.Text.Length > 0 
        ? BannerTextMode.EditMode 
        : BannerTextMode.FocusMode;

    var banner = new BannerTextInfo()
    {
        Text = "Required field",
        Visible = true,
        Mode = mode
    };

    bannerTextProvider1.SetBannerText(textBox, banner);
}
```

---

**Next:** See [supported-controls.md](supported-controls.md) for compatible control types
