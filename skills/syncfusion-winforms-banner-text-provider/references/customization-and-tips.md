# Customization and Tips

## Table of Contents
- [Advanced Styling](#advanced-styling)
- [Performance Optimization](#performance-optimization)
- [Common Pitfalls](#common-pitfalls)
- [Troubleshooting](#troubleshooting)
- [Best Practices Summary](#best-practices-summary)

## Advanced Styling

### Theme Integration

Apply banners that match your application theme:

```csharp
// Define theme-consistent banners
private void ApplyThemedBanners()
{
    Color themeAccent = Color.FromArgb(30, 144, 255);      // Dodger blue
    Color themeSecondary = Color.FromArgb(192, 192, 192);  // Silver

    var themedBanner = new BannerTextInfo()
    {
        Text = "Themed hint text",
        Visible = true,
        Color = themeAccent,
        Font = new Font("Segoe UI", 9, FontStyle.Italic)
    };

    bannerProvider.SetBannerText(textBox, themedBanner);
}
```

### Dark Mode Support

Adapt banner colors for dark themes:

```csharp
private void SetDarkModeTheme()
{
    // Dark background requires lighter text
    Color darkModeText = Color.FromArgb(200, 200, 200);  // Light gray

    var darkBanner = new BannerTextInfo()
    {
        Text = "Hint for dark theme",
        Visible = true,
        Color = darkModeText,
        Font = new Font("Segoe UI", 9, FontStyle.Italic)
    };

    bannerProvider.SetBannerText(textBox, darkBanner);
}
```

### Custom Font Loading

Use custom fonts for special effects:

```csharp
try
{
    // Load custom font from resources
    var fontCollection = new System.Drawing.Text.PrivateFontCollection();
    fontCollection.AddFontFile("path/to/custom-font.ttf");
    var customFont = new Font(fontCollection.Families[0], 9, FontStyle.Italic);

    var customBanner = new BannerTextInfo()
    {
        Text = "Custom font banner",
        Visible = true,
        Font = customFont,
        Color = SystemColors.GrayText
    };

    bannerProvider.SetBannerText(textBox, customBanner);
}
catch (Exception ex)
{
    // Fallback to system font if custom font fails
    MessageBox.Show("Font loading error: " + ex.Message);
}
```

### Conditional Banner Styling

Apply different styles based on context:

```csharp
private void SetContextualBanners(bool isReadOnly)
{
    Color bannerColor = isReadOnly ? Color.LightGray : SystemColors.GrayText;
    FontStyle bannerStyle = isReadOnly ? FontStyle.Regular : FontStyle.Italic;

    var banner = new BannerTextInfo()
    {
        Text = "Enter value",
        Visible = true,
        Color = bannerColor,
        Font = new Font("Arial", 9, bannerStyle)
    };

    bannerProvider.SetBannerText(textBox, banner);
}
```

## Performance Optimization

### Batch Banner Setup

For forms with many controls, batch setup improves performance:

```csharp
private void SetupMultipleBannersBatch()
{
    var controls = new[] { textBox1, textBox2, textBox3, comboBox1 };
    var hints = new[] { "Name", "Email", "Phone", "Category" };

    // Pre-create banner style for reuse
    var font = new Font("Segoe UI", 9, FontStyle.Italic);
    var color = SystemColors.GrayText;

    for (int i = 0; i < controls.Length; i++)
    {
        var banner = new BannerTextInfo()
        {
            Text = hints[i],
            Visible = true,
            Font = font,
            Color = color
        };

        bannerProvider.SetBannerText(controls[i], banner);
    }

    // Reuse font object to minimize memory allocation
}
```

### Lazy Banner Initialization

Defer banner setup until tab/panel is accessed:

```csharp
private bool advancedTabInitialized = false;

private void AdvancedTabControl_SelectedIndexChanged(object sender, EventArgs e)
{
    if (advancedTabControl.SelectedIndex == 2 && !advancedTabInitialized)
    {
        // Initialize banners only when tab is viewed
        InitializeAdvancedBanners();
        advancedTabInitialized = true;
    }
}

private void InitializeAdvancedBanners()
{
    var banner = new BannerTextInfo()
    {
        Text = "Advanced field...",
        Visible = true,
        Mode = BannerTextMode.EditMode
    };

    bannerProvider.SetBannerText(advancedTextBox, banner);
}
```

### Shared Font Objects

Reuse Font objects across multiple banners:

```csharp
private Font labelFont;      // Reusable Font objects
private Font placeholderFont;

private void Form_Load(object sender, EventArgs e)
{
    // Create fonts once
    labelFont = new Font("Arial", 10, FontStyle.Bold);
    placeholderFont = new Font("Arial", 9, FontStyle.Italic);

    // Reuse across multiple banners
    for (int i = 0; i < 20; i++)
    {
        var banner = new BannerTextInfo()
        {
            Text = "Placeholder " + i,
            Font = placeholderFont,  // Reuse
            Color = SystemColors.GrayText
        };

        bannerProvider.SetBannerText(controls[i], banner);
    }
}

private void Form_FormClosing(object sender, FormClosingEventArgs e)
{
    // Dispose reusable resources
    labelFont?.Dispose();
    placeholderFont?.Dispose();
}
```

## Common Pitfalls

### ❌ Pitfall 1: Not Clearing Default Text

**Problem:** Banner text doesn't display because control already has text

```csharp
// WRONG
textBox.Text = "Some default value";
bannerProvider.SetBannerText(textBox, 
    new BannerTextInfo("Banner text", true));
// Result: Banner not visible because text box has content
```

**Solution:**

```csharp
// CORRECT
textBox.Text = "";  // Clear first
bannerProvider.SetBannerText(textBox, 
    new BannerTextInfo("Banner text", true));
// Result: Banner displays correctly
```

### ❌ Pitfall 2: Wrong Mode Choice

**Problem:** Choosing FocusMode when EditMode is needed

```csharp
// WRONG for required field
var banner = new BannerTextInfo()
{
    Text = "Email (required)",
    Mode = BannerTextMode.FocusMode  // User might forget requirement
};

// User focuses field → banner disappears → user doesn't remember it's required
```

**Solution:**

```csharp
// CORRECT
var banner = new BannerTextInfo()
{
    Text = "Email (required)",
    Mode = BannerTextMode.EditMode  // Banner stays visible until typed
};

// User sees hint while editing
```

### ❌ Pitfall 3: Incompatible Control

**Problem:** Using banner on unsupported control

```csharp
// WRONG - RichTextBox not supported
bannerProvider.SetBannerText(richTextBox, 
    new BannerTextInfo("This won't work", true));
// Result: Banner doesn't appear
```

**Solution:** Check [supported-controls.md](supported-controls.md) or use supported alternatives

### ❌ Pitfall 4: Not Disposing BannerTextProvider

**Problem:** Memory leak from unreleased resources

```csharp
// WRONG
private void Form_Load(object sender, EventArgs e)
{
    bannerProvider = new BannerTextProvider(this.components);
    // If passed null instead of this.components, memory leak
}
```

**Solution:**

```csharp
// CORRECT
private void Form_Load(object sender, EventArgs e)
{
    bannerProvider = new BannerTextProvider(this.components);
    // Pass this.components for proper cleanup
}

// Form's Dispose handles cleanup automatically
protected override void Dispose(bool disposing)
{
    if (disposing)
    {
        // this.components.Dispose() handles bannerProvider
    }
    base.Dispose(disposing);
}
```

### ❌ Pitfall 5: Conflicting Text and Banner

**Problem:** Banner text conflicts with control's Text property

```csharp
// WRONG
textBox.Text = "Placeholder";  // Sets control Text
bannerProvider.SetBannerText(textBox, 
    new BannerTextInfo("Banner text", true));
// Result: Confusing, both displayed incorrectly
```

**Solution:**

```csharp
// CORRECT
textBox.Text = "";  // Keep control Text empty
bannerProvider.SetBannerText(textBox, 
    new BannerTextInfo("Placeholder", true));
// Result: Clean, only banner displays
```

## Troubleshooting

### Issue: Banner Text Not Showing

**Cause 1: Control already has text**
```csharp
// Fix: Clear the control
textBox.Text = "";
```

**Cause 2: Visible property is false**
```csharp
// Fix: Set Visible to true
var banner = new BannerTextInfo()
{
    Text = "Your text",
    Visible = true  // Must be true
};
```

**Cause 3: Unsupported control type**
```csharp
// Fix: Use supported control
// Replace RichTextBox with TextBoxExt
bannerProvider.SetBannerText(textBoxExt, banner);
```

**Cause 4: Color matches background**
```csharp
// Fix: Use contrasting color
var banner = new BannerTextInfo()
{
    Text = "Your text",
    Color = Color.Gray  // Contrast with white background
};
```

### Issue: Banner Disappears Unexpectedly

**Cause 1: Mode set to FocusMode**
```csharp
// Banner disappears when control receives focus
// Fix: Use EditMode if you want persistent display
var banner = new BannerTextInfo()
{
    Text = "Text",
    Mode = BannerTextMode.EditMode
};
```

**Cause 2: Focus changes during validation**
```csharp
// ValidationEventArgs might trigger focus loss
// Fix: Update banner after validation complete
private void TextBox_Validating(object sender, CancelEventArgs e)
{
    bool isValid = ValidateInput();
    
    // Update banner based on validation
    var newBanner = new BannerTextInfo()
    {
        Text = isValid ? "Valid" : "Invalid",
        Visible = true
    };
    
    bannerProvider.SetBannerText(textBox, newBanner);
}
```

### Issue: Multiple Banners Conflict

**Cause: Applying banner twice without clearing**
```csharp
// WRONG
bannerProvider.SetBannerText(textBox, banner1);
bannerProvider.SetBannerText(textBox, banner2);  // Overwrites banner1
```

**Fix: Only set once, or explicitly replace**
```csharp
// CORRECT - Set single time
bannerProvider.SetBannerText(textBox, banner);

// Or explicitly replace
bannerProvider.SetBannerText(textBox, newBanner);  // Replaces previous
```

### Issue: Performance Degradation

**Cause: Creating too many Font objects**
```csharp
// WRONG - Creates new Font for each banner
for (int i = 0; i < 100; i++)
{
    var banner = new BannerTextInfo()
    {
        Font = new Font("Arial", 9)  // Memory leak
    };
}
```

**Fix: Reuse Font objects**
```csharp
// CORRECT
var font = new Font("Arial", 9);
for (int i = 0; i < 100; i++)
{
    var banner = new BannerTextInfo()
    {
        Font = font  // Reuse same font
    };
}
```

## Best Practices Summary

✅ **Do:**
- Clear control's Text property before setting banner
- Use EditMode for required fields
- Use FocusMode for optional hints
- Match color to your theme
- Combine font styling (italic) with color for accessibility
- Reuse Font objects for performance
- Pass `this.components` when creating BannerTextProvider
- Test with all target editor controls
- Document banner text strategy in code comments

❌ **Don't:**
- Leave control's Text property set
- Use banner on unsupported controls
- Create Font objects in loops
- Rely on color alone for distinction
- Forget to dispose BannerTextProvider
- Use very long banner text (displays cut off)
- Mix control Text with banner text
- Change banner mode too frequently at runtime

### Recommended Setup Checklist

```csharp
public class FormSetupChecklist
{
    // ✓ Create BannerTextProvider with this.components
    // ✓ Clear all control Text properties
    // ✓ Create Font objects once, reuse
    // ✓ Choose correct Mode per field
    // ✓ Test color contrast
    // ✓ Handle dynamic updates cleanly
    // ✓ Document banner strategy
    // ✓ Test on target systems
}
```

---

**Reference complete.** For more implementation patterns, see [practical-examples.md](practical-examples.md)
