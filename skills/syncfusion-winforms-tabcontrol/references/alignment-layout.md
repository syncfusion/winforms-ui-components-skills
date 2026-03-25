# Alignment and Layout

Configure tab positioning, orientation, text alignment, and layout options for TabControlAdv.

## Tab Alignment

Control where tabs are displayed relative to the tab content area.

### Alignment Property

```csharp
// Tabs at top (default)
tabControlAdv1.Alignment = TabAlignment.Top;

// Tabs at bottom
tabControlAdv1.Alignment = TabAlignment.Bottom;

// Tabs on left side (vertical)
tabControlAdv1.Alignment = TabAlignment.Left;

// Tabs on right side (vertical)
tabControlAdv1.Alignment = TabAlignment.Right;
```

### TabGap Property

Set the spacing between tabs:

```csharp
// 2 pixels between tabs
tabControlAdv1.TabGap = 2;

// 5 pixels for more spacing
tabControlAdv1.TabGap = 5;

// No gap
tabControlAdv1.TabGap = 0;
```

## Text Alignment

Control how text is aligned within each tab.

### TextAlignment Property

Horizontal alignment of text:

```csharp
// Text aligned to the left
tabControlAdv1.TextAlignment = StringAlignment.Near;

// Text centered (default)
tabControlAdv1.TextAlignment = StringAlignment.Center;

// Text aligned to the right
tabControlAdv1.TextAlignment = StringAlignment.Far;
```

### TextLineAlignment Property

Vertical alignment of text:

```csharp
// Text at top of tab
tabControlAdv1.TextLineAlignment = StringAlignment.Near;

// Text centered vertically (default)
tabControlAdv1.TextLineAlignment = StringAlignment.Center;

// Text at bottom of tab
tabControlAdv1.TextLineAlignment = StringAlignment.Far;
```

### Combined Text Alignment Example

```csharp
// Bottom-right aligned text
tabControlAdv1.TextAlignment = StringAlignment.Far;
tabControlAdv1.TextLineAlignment = StringAlignment.Far;
```

## Multiline Support

Arrange tabs in multiple rows or wrap text within tabs.

### Multiline Tabs

Enable multiple rows of tabs when they exceed the control width:

```csharp
// Enable multiple rows of tabs
tabControlAdv1.Multiline = true;

// Disable multiline (single row with scrolling)
tabControlAdv1.Multiline = false;
```

**When to use:** Add many tabs that don't fit in a single row.

### MultilineText Property

Allow text within a single tab to wrap to multiple lines:

```csharp
// Enable text wrapping in tabs
tabControlAdv1.MultilineText = true;

// Example with long text
TabPageAdv tab = new TabPageAdv();
tab.Text = "This is a very long tab title that will wrap";
tabControlAdv1.TabPages.Add(tab);
```

### KeepSelectedTabInFrontRow

When using multiline tabs, bring the selected tab to the front row:

```csharp
// Keep selected tab in front row
tabControlAdv1.KeepSelectedTabInFrontRow = true;
```

**Example:**
```csharp
TabControlAdv tabControl = new TabControlAdv();
tabControl.Multiline = true;
tabControl.KeepSelectedTabInFrontRow = true;
tabControl.Size = new Size(400, 300);

// Add 15 tabs - they'll wrap to multiple rows
for (int i = 1; i <= 15; i++)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = $"Tab {i}";
    tabControl.TabPages.Add(tab);
}
```

## RTL (Right-to-Left) Support

Support for right-to-left languages like Arabic and Hebrew.

### RightToLeft Property

Enable RTL mode:

```csharp
// Enable RTL layout
tabControlAdv1.RightToLeft = RightToLeft.Yes;

// Standard LTR layout
tabControlAdv1.RightToLeft = RightToLeft.No;
```

**Effect:**
- Tabs and text are drawn from right to left
- Tab order is reversed
- Useful for Arabic, Hebrew, and other RTL languages

### RotateTabsWhenRTL Property

Rotate tabs when aligned to left or right sides in RTL mode:

```csharp
// Enable tab rotation in RTL mode
tabControlAdv1.RightToLeft = RightToLeft.Yes;
tabControlAdv1.RotateTabsWhenRTL = true;
```

**Requirements:**
- `RightToLeft` must be set to `Yes`
- Only affects tabs aligned to left or right
- Default value is `false`

**Example:**
```csharp
TabControlAdv rtlTabs = new TabControlAdv();
rtlTabs.RightToLeft = RightToLeft.Yes;
rtlTabs.Alignment = TabAlignment.Right;
rtlTabs.RotateTabsWhenRTL = true;

TabPageAdv arabicTab = new TabPageAdv();
arabicTab.Text = "الرئيسية"; // "Home" in Arabic
rtlTabs.TabPages.Add(arabicTab);
```

## Rotating Tabs and Text

Control text orientation for vertically-aligned tabs.

### RotateTextWhenVertical Property

Rotate tab text 90 degrees when tabs are on left or right:

```csharp
// Enable text rotation for vertical tabs
tabControlAdv1.RotateTextWhenVertical = true;

// Example with left-aligned tabs
tabControlAdv1.Alignment = TabAlignment.Left;
tabControlAdv1.RotateTextWhenVertical = true;
```

**When to use:** 
- Improves readability for left/right aligned tabs
- Text reads top-to-bottom instead of sideways

**Example:**
```csharp
// Vertical tabs with rotated text
TabControlAdv verticalTabs = new TabControlAdv();
verticalTabs.Alignment = TabAlignment.Left;
verticalTabs.RotateTextWhenVertical = true;
verticalTabs.Size = new Size(500, 400);

TabPageAdv tab1 = new TabPageAdv();
tab1.Text = "Dashboard";

TabPageAdv tab2 = new TabPageAdv();
tab2.Text = "Reports";

TabPageAdv tab3 = new TabPageAdv();
tab3.Text = "Settings";

verticalTabs.TabPages.Add(tab1);
verticalTabs.TabPages.Add(tab2);
verticalTabs.TabPages.Add(tab3);
```

## Complete Layout Examples

### Example 1: Bottom-Aligned Tabs with Gaps

```csharp
TabControlAdv bottomTabs = new TabControlAdv();
bottomTabs.Alignment = TabAlignment.Bottom;
bottomTabs.TabGap = 3;
bottomTabs.Multiline = false;
bottomTabs.Size = new Size(600, 400);

for (int i = 1; i <= 5; i++)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = $"Document {i}";
    bottomTabs.TabPages.Add(tab);
}

this.Controls.Add(bottomTabs);
```

### Example 2: Left-Aligned Multiline Tabs

```csharp
TabControlAdv leftTabs = new TabControlAdv();
leftTabs.Alignment = TabAlignment.Left;
leftTabs.Multiline = true;
leftTabs.RotateTextWhenVertical = true;
leftTabs.Size = new Size(600, 400);

string[] categories = { "Home", "View", "Insert", "Design", 
                        "Layout", "References", "Mailings", "Review" };

foreach (string category in categories)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = category;
    leftTabs.TabPages.Add(tab);
}

this.Controls.Add(leftTabs);
```

### Example 3: RTL Layout for Arabic Application

```csharp
TabControlAdv arabicTabs = new TabControlAdv();
arabicTabs.RightToLeft = RightToLeft.Yes;
arabicTabs.Alignment = TabAlignment.Top;
arabicTabs.Size = new Size(600, 400);

TabPageAdv homeTab = new TabPageAdv();
homeTab.Text = "الرئيسية"; // Home

TabPageAdv settingsTab = new TabPageAdv();
settingsTab.Text = "الإعدادات"; // Settings

TabPageAdv helpTab = new TabPageAdv();
helpTab.Text = "مساعدة"; // Help

arabicTabs.TabPages.Add(homeTab);
arabicTabs.TabPages.Add(settingsTab);
arabicTabs.TabPages.Add(helpTab);

this.Controls.Add(arabicTabs);
```

### Example 4: Multiline with Text Wrapping

```csharp
TabControlAdv wrapTabs = new TabControlAdv();
wrapTabs.Multiline = true;
wrapTabs.MultilineText = true;
wrapTabs.KeepSelectedTabInFrontRow = true;
wrapTabs.Size = new Size(400, 350);

// Tabs with long text that will wrap
TabPageAdv tab1 = new TabPageAdv();
tab1.Text = "Customer Information Management";

TabPageAdv tab2 = new TabPageAdv();
tab2.Text = "Sales Reports and Analytics";

TabPageAdv tab3 = new TabPageAdv();
tab3.Text = "Inventory Tracking System";

wrapTabs.TabPages.Add(tab1);
wrapTabs.TabPages.Add(tab2);
wrapTabs.TabPages.Add(tab3);

this.Controls.Add(wrapTabs);
```

## Best Practices

### Tab Alignment
- **Top:** Most common, familiar to users
- **Bottom:** Good for toolbar-like interfaces
- **Left/Right:** Use for settings dialogs or categorized navigation
- Enable `RotateTextWhenVertical` for better readability with vertical tabs

### Multiline Tabs
- Use when you have many tabs (>8)
- Consider scrolling as an alternative for better space usage
- Set `KeepSelectedTabInFrontRow = true` for better visibility

### RTL Support
- Test with actual RTL text
- Use `RotateTabsWhenRTL` for vertical tabs
- Ensure all UI elements support RTL consistently

### Text Alignment
- Center alignment works best for most scenarios
- Use custom alignment to match specific design requirements
- Consider tab width when choosing alignment

## Common Scenarios

### Scenario 1: Ribbon-Style Interface
```csharp
// Top-aligned tabs with no gap for seamless look
tabControlAdv1.Alignment = TabAlignment.Top;
tabControlAdv1.TabGap = 0;
tabControlAdv1.Multiline = false;
```

### Scenario 2: Settings Dialog
```csharp
// Left-aligned vertical tabs
tabControlAdv1.Alignment = TabAlignment.Left;
tabControlAdv1.RotateTextWhenVertical = true;
tabControlAdv1.TabGap = 2;
```

### Scenario 3: Document Tabs
```csharp
// Top tabs with multiline support
tabControlAdv1.Alignment = TabAlignment.Top;
tabControlAdv1.Multiline = true;
tabControlAdv1.KeepSelectedTabInFrontRow = true;
```
