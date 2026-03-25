# Appearance & Styling Customization

## Table of Contents
- [Tab Colors](#tab-colors)
- [Tab Fonts](#tab-fonts)
- [Icons in Tabs](#icons-in-tabs)
- [Panel Customization](#panel-customization)
- [Complete Examples](#complete-examples)

## Tab Colors

### Background Colors

Control the appearance of active and inactive tabs:

```csharp
// Active tab (currently displayed)
tabbedMDIManager.ActiveTabBackColor = Color.Blue;

// Inactive tabs (other documents)
tabbedMDIManager.TabBackColor = Color.LightGray;

// Tab panel background (behind the tabs)
tabbedMDIManager.TabPanelBackColor = Color.White;

// Tab panel border
tabbedMDIManager.TabPanelBorderColor = Color.Black;
```

### Text Colors

Customize text appearance for tabs:

```csharp
// Active tab text color
tabbedMDIManager.ActiveTabForeColor = Color.White;

// Inactive tab text color
tabbedMDIManager.TabForeColor = Color.Black;
```

### Color Scheme Example

```csharp
private void ApplyColorScheme()
{
    // Professional blue scheme
    tabbedMDIManager.ActiveTabBackColor = Color.FromArgb(0, 102, 204);    // Professional blue
    tabbedMDIManager.ActiveTabForeColor = Color.White;

    tabbedMDIManager.TabBackColor = Color.FromArgb(230, 230, 230);       // Light gray
    tabbedMDIManager.TabForeColor = Color.FromArgb(64, 64, 64);          // Dark gray text

    tabbedMDIManager.TabPanelBackColor = Color.White;
    tabbedMDIManager.TabPanelBorderColor = Color.FromArgb(200, 200, 200); // Subtle border
}

// Visual result:
// ┌────────────────────────────────────────┐
// │ Doc1 │ Doc2 │ Doc3                     │  ← Light gray inactive tabs
// ├────────────────────────────────────────┤
// │                                        │
// │  Blue active tab with white text       │
// │                                        │
// └────────────────────────────────────────┘
```

## Tab Fonts

### Setting Tab Fonts

Control font appearance for active and inactive tabs:

```csharp
// Active tab font (highlighted)
tabbedMDIManager.ActiveTabFont = new Font("Arial", 11, FontStyle.Bold);

// Inactive tab font (regular)
tabbedMDIManager.TabFont = new Font("Arial", 10, FontStyle.Regular);
```

### Font Style Examples

```csharp
private void SetDifferentFonts()
{
    // Modern style
    tabbedMDIManager.ActiveTabFont = new Font("Segoe UI", 10, FontStyle.Bold | FontStyle.Underline);
    tabbedMDIManager.TabFont = new Font("Segoe UI", 10, FontStyle.Regular);

    // Traditional style
    tabbedMDIManager.ActiveTabFont = new Font("Times New Roman", 11, FontStyle.Bold | FontStyle.Italic);
    tabbedMDIManager.TabFont = new Font("Times New Roman", 10, FontStyle.Regular);

    // Classic style
    tabbedMDIManager.ActiveTabFont = new Font("Courier New", 10, FontStyle.Bold);
    tabbedMDIManager.TabFont = new Font("Courier New", 10, FontStyle.Regular);
}
```

## Icons in Tabs

### Enable Icons

Display icons next to tab text:

```csharp
// Enable icon display
tabbedMDIManager.UseIconsInTabs = true;

// Set icon size (width, height)
tabbedMDIManager.ImageSize = new Size(16, 16);  // Recommended: 16x16 or 32x32
```

### Assigning Icons to Forms

```csharp
private void CreateDocumentWithIcon()
{
    Form doc = new Form();
    doc.Text = "Document 1";
    doc.MdiParent = this;

    // Assign icon
    try
    {
        doc.Icon = new Icon("document.ico");
    }
    catch
    {
        // Fallback if icon not found
        doc.Icon = SystemIcons.WinLogo;
    }

    doc.Show();
}

// Visual result with icons enabled:
// ┌──────────────────────────────────────┐
// │ 📄 Doc1 │ 📊 Doc2 │ 📝 Doc3          │
// └──────────────────────────────────────┘
```

### Icon Size Comparison

```csharp
// Small icons (16x16)
tabbedMDIManager.ImageSize = new Size(16, 16);
// ┌──────────────────┐
// │ 🗒 Doc1 │ 🗒 Doc2 │  ← Compact
// └──────────────────┘

// Large icons (32x32)
tabbedMDIManager.ImageSize = new Size(32, 32);
// ┌─────────────────────────┐
// │ 📄 Doc1 │ 📄 Doc2      │  ← More visible
// └─────────────────────────┘
```

## Panel Customization

### Complete Tab Panel Control

```csharp
private void CustomizeTabPanel()
{
    // Background
    tabbedMDIManager.TabPanelBackColor = Color.FromArgb(245, 245, 245);

    // Border
    tabbedMDIManager.TabPanelBorderColor = Color.FromArgb(200, 200, 200);

    // This customizes the area at the bottom/right/left of the tab strip
    // (depending on tab alignment)
}
```

## Complete Examples

### Example 1: Dark Theme

```csharp
private void ApplyDarkTheme()
{
    // Dark background
    tabbedMDIManager.TabPanelBackColor = Color.FromArgb(45, 45, 45);

    // Active tab - bright highlight
    tabbedMDIManager.ActiveTabBackColor = Color.FromArgb(0, 120, 215);
    tabbedMDIManager.ActiveTabForeColor = Color.White;

    // Inactive tabs - subtle
    tabbedMDIManager.TabBackColor = Color.FromArgb(60, 60, 60);
    tabbedMDIManager.TabForeColor = Color.FromArgb(200, 200, 200);

    // Fonts
    tabbedMDIManager.ActiveTabFont = new Font("Segoe UI", 10, FontStyle.Bold);
    tabbedMDIManager.TabFont = new Font("Segoe UI", 10, FontStyle.Regular);

    // Border
    tabbedMDIManager.TabPanelBorderColor = Color.FromArgb(30, 30, 30);
}

// Visual result:
// ┌─────────────────────────────────────┐
// │  Doc1 │  Doc2 │  Doc3               │  ← Light gray text on dark
// ├─────────────────────────────────────┤
// │                                     │
// │            Content Area             │
// │            (Dark Theme)             │
// │                                     │
// └─────────────────────────────────────┘
```

### Example 2: Light Professional Theme

```csharp
private void ApplyProfessionalTheme()
{
    // Clean white background
    tabbedMDIManager.TabPanelBackColor = Color.White;
    tabbedMDIManager.TabPanelBorderColor = Color.FromArgb(220, 220, 220);

    // Active - professional blue
    tabbedMDIManager.ActiveTabBackColor = Color.FromArgb(0, 102, 204);
    tabbedMDIManager.ActiveTabForeColor = Color.White;

    // Inactive - neutral gray
    tabbedMDIManager.TabBackColor = Color.FromArgb(240, 240, 240);
    tabbedMDIManager.TabForeColor = Color.FromArgb(80, 80, 80);

    // Professional fonts
    var regularFont = new Font("Segoe UI", 10);
    var boldFont = new Font("Segoe UI", 10, FontStyle.Bold);

    tabbedMDIManager.TabFont = regularFont;
    tabbedMDIManager.ActiveTabFont = boldFont;
}

// Visual result (professional office appearance):
// ┌─────────────────────────────────────┐
// │ Doc1 │ Doc2 │ Doc3                  │  ← Clean gray tabs
// ├─────────────────────────────────────┤
// │ 📄 Active Document                  │  ← Blue header
// │ Content goes here...                │
// │                                     │
// └─────────────────────────────────────┘
```

### Example 3: High-Contrast Theme (Accessibility)

```csharp
private void ApplyHighContrastTheme()
{
    // Maximum contrast for accessibility
    tabbedMDIManager.TabPanelBackColor = Color.White;
    tabbedMDIManager.TabPanelBorderColor = Color.Black;

    // Active - strong contrast
    tabbedMDIManager.ActiveTabBackColor = Color.Black;
    tabbedMDIManager.ActiveTabForeColor = Color.Yellow;

    // Inactive - clear distinction
    tabbedMDIManager.TabBackColor = Color.White;
    tabbedMDIManager.TabForeColor = Color.Black;

    // Bold fonts for readability
    tabbedMDIManager.TabFont = new Font("Arial", 12, FontStyle.Bold);
    tabbedMDIManager.ActiveTabFont = new Font("Arial", 12, FontStyle.Bold | FontStyle.Underline);
}

// Visual result (high contrast):
// ┌──────────────────────────────────────┐
// │ Doc1 │ Doc2 │ Doc3                   │  ← White with black text
// ├──────────────────────────────────────┤
// │ 📄 Active Document                   │  ← Bold text, clear borders
// │ Content is highly readable...        │
// │                                      │
// └──────────────────────────────────────┘
```

### Example 4: Theme Switcher

```csharp
public partial class ThemeSwitcherForm : Form
{
    private TabbedMDIManager tabbedMDI;

    public ThemeSwitcherForm()
    {
        InitializeComponent();
        SetupUI();
    }

    private void SetupUI()
    {
        this.IsMdiContainer = true;
        this.Text = "Theme Switcher Demo";
        this.Size = new Size(900, 600);

        tabbedMDI = new TabbedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);

        CreateThemeMenu();
        CreateSampleDocuments();
    }

    private void CreateThemeMenu()
    {
        MenuStrip menu = new MenuStrip();
        this.Controls.Add(menu);
        this.MainMenuStrip = menu;

        ToolStripMenuItem viewMenu = menu.Items.Add("&View") as ToolStripMenuItem;
        viewMenu.DropDownItems.Add("&Dark Theme", null, (s, e) => ApplyDarkTheme());
        viewMenu.DropDownItems.Add("&Professional Theme", null, (s, e) => ApplyProfessionalTheme());
        viewMenu.DropDownItems.Add("&Light Theme", null, (s, e) => ApplyLightTheme());
        viewMenu.DropDownItems.Add("&High Contrast", null, (s, e) => ApplyHighContrastTheme());
    }

    private void ApplyDarkTheme()
    {
        tabbedMDI.TabPanelBackColor = Color.FromArgb(45, 45, 45);
        tabbedMDI.ActiveTabBackColor = Color.FromArgb(0, 120, 215);
        tabbedMDI.ActiveTabForeColor = Color.White;
        tabbedMDI.TabBackColor = Color.FromArgb(60, 60, 60);
        tabbedMDI.TabForeColor = Color.FromArgb(200, 200, 200);
    }

    private void ApplyProfessionalTheme()
    {
        tabbedMDI.TabPanelBackColor = Color.White;
        tabbedMDI.ActiveTabBackColor = Color.FromArgb(0, 102, 204);
        tabbedMDI.ActiveTabForeColor = Color.White;
        tabbedMDI.TabBackColor = Color.FromArgb(240, 240, 240);
        tabbedMDI.TabForeColor = Color.FromArgb(80, 80, 80);
    }

    private void ApplyLightTheme()
    {
        tabbedMDI.TabPanelBackColor = Color.FromArgb(250, 250, 250);
        tabbedMDI.ActiveTabBackColor = Color.FromArgb(100, 150, 200);
        tabbedMDI.ActiveTabForeColor = Color.White;
        tabbedMDI.TabBackColor = Color.FromArgb(220, 220, 220);
        tabbedMDI.TabForeColor = Color.Black;
    }

    private void ApplyHighContrastTheme()
    {
        tabbedMDI.TabPanelBackColor = Color.White;
        tabbedMDI.ActiveTabBackColor = Color.Black;
        tabbedMDI.ActiveTabForeColor = Color.Yellow;
        tabbedMDI.TabBackColor = Color.White;
        tabbedMDI.TabForeColor = Color.Black;
    }

    private void CreateSampleDocuments()
    {
        for (int i = 1; i <= 3; i++)
        {
            Form doc = new Form();
            doc.Text = $"Document {i}";
            doc.MdiParent = this;
            doc.Show();
        }
    }
}
```

## Best Practices

1. **Consistency** - Use matching color schemes across your application
2. **Contrast** - Ensure sufficient contrast between active and inactive tabs
3. **Accessibility** - Provide high-contrast option for visually impaired users
4. **Font sizes** - Use 10-12 pt for readability on most screens
5. **Icons** - Use 16x16 or 32x32 for consistent appearance
6. **User preference** - Consider saving user's theme choice

## Troubleshooting

### Issue: Colors Not Changing
**Solution:** Ensure you set the properties BEFORE showing documents or handle `TabControlAdded` event

### Issue: Font Looks Different Than Expected
**Solution:** Verify the font is installed on the system. Use `Segoe UI` as safe default

### Issue: Icons Not Displaying
**Solution:** Ensure:
- `UseIconsInTabs = true`
- Form has valid `Icon` property set
- Icon file exists and is accessible
