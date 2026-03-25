# Supported Controls

## Table of Contents
- [Complete Control List](#complete-control-list)
- [Control Categories](#control-categories)
- [Compatibility Matrix](#compatibility-matrix)
- [Best Practices by Control Type](#best-practices-by-control-type)
- [Unsupported Controls](#unsupported-controls)

## Complete Control List

BannerTextProvider supports 14+ controls across multiple Syncfusion and Microsoft component categories:

### Primary Editor Controls (Recommended)

1. **TextBoxExt** - Extended text box (Syncfusion)
2. **CurrencyTextBox** - Currency input control (Syncfusion)
3. **ComboBoxAdv** - Advanced combo box (Syncfusion)
4. **ComboDropDown** - Specialized combo box (Syncfusion)
5. **ComboBoxAutoComplete** - Auto-complete combo (Syncfusion)
6. **IntegerTextBox** - Integer-only input (Syncfusion)
7. **DoubleTextBox** - Decimal input (Syncfusion)
8. **PercentTextBox** - Percentage input (Syncfusion)

### Ribbon and Menu Controls

9. **TextBoxBarItem** - Text box in XPMenus
10. **ComboBoxBarItem** - Combo box in XPMenus
11. **ToolStripTextBox** - Text box in ToolStripEx (Ribbon)
12. **ToolStripComboBox** - Combo box in ToolStripEx (Ribbon)
13. **ToolStripComboBoxEx** - Extended combo in ToolStripEx (Ribbon)

### Microsoft Standard Controls

14. **TextBox** - Standard .NET TextBox
15. **ComboBox** - Standard .NET ComboBox
16. **Other Microsoft Editor Controls** - Various .NET editor controls

## Control Categories

### Category 1: Numeric Input Controls

**IntegerTextBox, DoubleTextBox, PercentTextBox, CurrencyTextBox**

```csharp
// Integer field
var integerBanner = new BannerTextInfo()
{
    Text = "Enter a whole number...",
    Visible = true,
    Mode = BannerTextMode.EditMode
};
bannerTextProvider1.SetBannerText(integerTextBox, integerBanner);

// Currency field
var currencyBanner = new BannerTextInfo()
{
    Text = "Enter amount (USD)...",
    Visible = true,
    Mode = BannerTextMode.EditMode,
    Color = Color.DarkGreen
};
bannerTextProvider1.SetBannerText(currencyTextBox, currencyBanner);
```

**Best for:** Numeric validation hints, unit/currency information

### Category 2: Text Input Controls

**TextBoxExt, TextBox**

```csharp
// Syncfusion TextBoxExt
var textBanner = new BannerTextInfo()
{
    Text = "Enter your name...",
    Visible = true,
    Font = new Font("Arial", 9, FontStyle.Italic)
};
bannerTextProvider1.SetBannerText(textBoxExt, textBanner);

// Standard TextBox
bannerTextProvider1.SetBannerText(standardTextBox, textBanner);
```

**Best for:** Names, descriptions, general text input

### Category 3: Dropdown and Selection Controls

**ComboBoxAdv, ComboDropDown, ComboBoxAutoComplete, ComboBox**

```csharp
// Advanced combo box
var comboBanner = new BannerTextInfo()
{
    Text = "Select an option...",
    Visible = true,
    Mode = BannerTextMode.FocusMode
};
bannerTextProvider1.SetBannerText(comboBoxAdv, comboBanner);
```

**Best for:** Selection hints, dropdown guidance

### Category 4: Ribbon and Menu Integration

**TextBoxBarItem, ComboBoxBarItem, ToolStripTextBox, ToolStripComboBox, ToolStripComboBoxEx**

```csharp
// In ToolStripEx (Ribbon context)
var toolstripBanner = new BannerTextInfo()
{
    Text = "Search...",
    Visible = true,
    Mode = BannerTextMode.FocusMode
};
bannerTextProvider1.SetBannerText(toolStripTextBox1, toolstripBanner);

// In XPMenus context
bannerTextProvider1.SetBannerText(textBoxBarItem, toolstripBanner);
```

**Best for:** Toolbar search fields, menu-integrated inputs

## Compatibility Matrix

| Control | Type | Recommended Mode | Notes |
|---------|------|------------------|-------|
| **TextBoxExt** | Text | Both | Full support, recommended |
| **IntegerTextBox** | Numeric | EditMode | Validates integer input |
| **DoubleTextBox** | Numeric | EditMode | Decimal support |
| **PercentTextBox** | Numeric | EditMode | Percentage formatting |
| **CurrencyTextBox** | Numeric | EditMode | Currency formatting |
| **ComboBoxAdv** | Dropdown | FocusMode | Advanced features |
| **ComboDropDown** | Dropdown | FocusMode | Specialized dropdown |
| **ComboBoxAutoComplete** | Dropdown | FocusMode | Auto-completion support |
| **ToolStripTextBox** | Ribbon | FocusMode | Space-constrained |
| **ToolStripComboBox** | Ribbon | FocusMode | Space-constrained |
| **TextBox** (Standard) | Text | Both | .NET Framework |
| **ComboBox** (Standard) | Dropdown | FocusMode | .NET Framework |

**Legend:**
- ✓ Full support, all features work
- ⚠ Limited support, some constraints
- ✗ No support

## Best Practices by Control Type

### For Numeric Input Controls

**Use EditMode** to keep the hint visible until user enters a number:

```csharp
var numericBanner = new BannerTextInfo()
{
    Text = "Whole numbers only",
    Visible = true,
    Mode = BannerTextMode.EditMode,
    Color = SystemColors.GrayText,
    Font = new Font("Arial", 8, FontStyle.Italic)
};

bannerTextProvider1.SetBannerText(integerTextBox, numericBanner);
```

**Why:** Users might hesitate with numeric fields; persistent guidance helps

### For Dropdown/Combo Controls

**Use FocusMode** to keep the dropdown uncluttered:

```csharp
var dropdownBanner = new BannerTextInfo()
{
    Text = "Choose one...",
    Visible = true,
    Mode = BannerTextMode.FocusMode
};

bannerTextProvider1.SetBannerText(comboBoxAdv, dropdownBanner);
```

**Why:** Dropdowns have limited space; FocusMode minimizes visual clutter

### For Text Input Controls

**Use EditMode for required fields, FocusMode for optional:**

```csharp
// Required field (EditMode)
var requiredBanner = new BannerTextInfo()
{
    Text = "Name (required)",
    Visible = true,
    Mode = BannerTextMode.EditMode,
    Color = Color.DarkRed
};
bannerTextProvider1.SetBannerText(nameTextBox, requiredBanner);

// Optional field (FocusMode)
var optionalBanner = new BannerTextInfo()
{
    Text = "Comments (optional)",
    Visible = true,
    Mode = BannerTextMode.FocusMode
};
bannerTextProvider1.SetBannerText(commentsTextBox, optionalBanner);
```

### For Ribbon/Menu Integration

**Use compact text and FocusMode:**

```csharp
var ribbonBanner = new BannerTextInfo()
{
    Text = "Search",
    Visible = true,
    Mode = BannerTextMode.FocusMode,
    Font = new Font("Arial", 8)  // Smaller for ribbon space
};

bannerTextProvider1.SetBannerText(toolStripTextBox, ribbonBanner);
```

**Why:** Ribbon controls have tight space constraints

## Multi-Control Application

### Apply to Multiple Controls Efficiently

```csharp
// Define control-to-placeholder mapping
var controlMap = new Dictionary<Control, string>()
{
    { nameTextBox, "Full Name" },
    { emailTextBox, "Email Address" },
    { phoneTextBox, "Phone Number (optional)" },
    { comboBoxStatus, "Select Status..." }
};

// Apply banners
foreach (var kvp in controlMap)
{
    var banner = new BannerTextInfo()
    {
        Text = kvp.Value,
        Visible = true,
        Mode = BannerTextMode.EditMode,
        Color = SystemColors.GrayText,
        Font = new Font("Verdana", 9, FontStyle.Italic)
    };

    bannerTextProvider1.SetBannerText(kvp.Key, banner);
}
```

### Form Template Pattern

Create a helper method for consistent banner application:

```csharp
private void SetBannerText(Control control, string text, 
    BannerTextMode mode = BannerTextMode.EditMode)
{
    var banner = new BannerTextInfo()
    {
        Text = text,
        Visible = true,
        Mode = mode,
        Color = SystemColors.GrayText,
        Font = new Font("Segoe UI", 9, FontStyle.Italic)
    };

    bannerTextProvider1.SetBannerText(control, banner);
}

// Usage
private void SetupForm()
{
    SetBannerText(nameTextBox, "Enter your full name");
    SetBannerText(emailTextBox, "user@example.com");
    SetBannerText(searchBox, "Search by keyword...", BannerTextMode.FocusMode);
}
```

## Unsupported Controls

The following controls do **NOT** support BannerTextProvider:

- **Label** - Static text display
- **Button** - Action controls
- **CheckBox** - Boolean selection
- **RadioButton** - Option selection
- **ListBox** - Multi-item selection
- **TreeView** - Hierarchical display
- **DataGridView** - Tabular data
- **RichTextBox** - Rich text (no banner support)
- **MaskedTextBox** - Input mask controls
- **NumericUpDown** - Spinner controls

**Workaround for RichTextBox:**

If you need placeholder text for RichTextBox, implement manual solution:

```csharp
private void RichTextBox_Enter(object sender, EventArgs e)
{
    if (richTextBox.Text == "Click to enter text...")
    {
        richTextBox.Text = "";
        richTextBox.ForeColor = Color.Black;
    }
}

private void RichTextBox_Leave(object sender, EventArgs e)
{
    if (richTextBox.Text == "")
    {
        richTextBox.Text = "Click to enter text...";
        richTextBox.ForeColor = Color.Gray;
    }
}
```

## Important Considerations

⚠️ **Clear control Text property** - Remove default text before setting banner
⚠️ **Test with all controls** - Behavior may vary slightly across control types
⚠️ **Accessibility** - Combine color changes with font styling (italic/bold)
⚠️ **Focus handling** - Different controls may have different focus behavior
⚠️ **ReadOnly controls** - Set banner text on ReadOnly controls before making them read-only

---

**Next:** See [practical-examples.md](practical-examples.md) for end-to-end implementation examples
