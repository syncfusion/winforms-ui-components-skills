# Themes and Styles

This guide covers the built-in theme options for the RadialMenu control, including how to apply them, when to use each theme, and best practices for theme selection.

## Style Property Overview

The `Style` property provides five pre-configured visual themes for RadialMenu, each designed to match different application aesthetics and Office design languages. These themes control the overall color scheme, visual effects, and styling of the menu.

**Available Themes:**
- **Default** - Standard Syncfusion theme with classic appearance
- **Office2016Colorful** - Vibrant, modern Office 2016 theme with color accents
- **Office2016White** - Clean, minimalist white theme
- **Office2016DarkGray** - Professional dark gray theme
- **Office2016Black** - High-contrast black theme for low-light environments

**Setting the Style Property:**

```csharp
// Set theme at design time or runtime
this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
```

## Default Theme

The Default theme provides a standard appearance with neutral colors, suitable for applications that don't require specific Office branding.

### Applying Default Theme

```csharp
// Apply Default theme
this.radialMenu1.Style = RadialMenuStyle.Default;
```

**Visual Characteristics:**
- Neutral gray color scheme
- Standard contrast levels
- Classic menu appearance
- Minimal visual effects
- Works well on light backgrounds

**Complete Example:**

```csharp
private void ApplyDefaultTheme()
{
    // Set Default theme
    this.radialMenu1.Style = RadialMenuStyle.Default;
    
    // Configure basic properties
    this.radialMenu1.Size = new Size(300, 300);
    this.radialMenu1.Location = new Point(100, 100);
    this.radialMenu1.Visible = true;
    this.radialMenu1.MenuVisibility = true;
    
    // Add menu items
    RadialMenuItem item1 = new RadialMenuItem();
    item1.Text = "New";
    
    RadialMenuItem item2 = new RadialMenuItem();
    item2.Text = "Open";
    
    RadialMenuItem item3 = new RadialMenuItem();
    item3.Text = "Save";
    
    this.radialMenu1.Items.Add(item1);
    this.radialMenu1.Items.Add(item2);
    this.radialMenu1.Items.Add(item3);
}
```

**When to Use Default Theme:**
- Legacy applications requiring classic appearance
- Internal business tools without specific branding
- Prototyping before final theme selection
- Applications supporting older Windows versions
- When custom styling will be applied later

**Result:**
A straightforward, professional menu with neutral styling that works in most contexts.

## Office2016Colorful Theme

The Office2016Colorful theme features vibrant colors and modern visual design, reflecting Microsoft Office 2016's colorful theme option.

### Applying Office2016Colorful Theme

```csharp
// Apply Office2016Colorful theme
this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
```

**Visual Characteristics:**
- Bright, vibrant color accents
- Modern, flat design language
- High visual contrast
- Colorful hover effects
- Energetic, contemporary appearance

**Complete Example:**

```csharp
private void ApplyOffice2016ColorfulTheme()
{
    // Set Office2016Colorful theme
    this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
    
    // Optimize for colorful theme
    this.radialMenu1.Size = new Size(320, 320);
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
    this.radialMenu1.MenuItemImageSize = new Size(28, 28);
    this.radialMenu1.WedgeCount = 6;
    
    // Set up colorful icons
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(LoadColorfulIcon("new"));
    imageList.Images.Add(LoadColorfulIcon("open"));
    imageList.Images.Add(LoadColorfulIcon("save"));
    imageList.Images.Add(LoadColorfulIcon("print"));
    
    this.radialMenu1.ImageList = imageList;
    
    // Add items
    string[] actions = { "New", "Open", "Save", "Print" };
    for (int i = 0; i < actions.Length; i++)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = actions[i];
        item.ImageIndex = i;
        item.Click += Action_Click;
        this.radialMenu1.Items.Add(item);
    }
}

private Image LoadColorfulIcon(string name)
{
    // Load or create vibrant icons that complement the theme
    return Image.FromFile($"icons/colorful/{name}.png");
}
```

**When to Use Office2016Colorful Theme:**
- Modern desktop applications targeting Windows 10/11
- Creative tools and design software
- Applications requiring visual energy and engagement
- Touch-enabled devices where color helps differentiation
- Younger or design-focused user demographics

**Best Practices:**
```csharp
// Pair with colorful, high-contrast icons
this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;  // Show both text and icons

// Use on light backgrounds for best effect
this.ParentForm.BackColor = Color.White;

// Ensure icons are vibrant and match theme energy
imageList.Images.Add(CreateVibriantIcon());
```

**Result:**
A lively, modern menu that stands out visually and attracts user attention.

## Office2016White Theme

The Office2016White theme provides a clean, minimalist appearance with white and light gray tones, ideal for distraction-free interfaces.

### Applying Office2016White Theme

```csharp
// Apply Office2016White theme
this.radialMenu1.Style = RadialMenuStyle.Office2016White;
```

**Visual Characteristics:**
- Clean white background
- Subtle gray accents
- Minimalist design
- Low visual weight
- Professional, uncluttered appearance

**Complete Example:**

```csharp
private void ApplyOffice2016WhiteTheme()
{
    // Set Office2016White theme
    this.radialMenu1.Style = RadialMenuStyle.Office2016White;
    
    // Configure for clean appearance
    this.radialMenu1.Size = new Size(300, 300);
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
    this.radialMenu1.MenuItemImageSize = new Size(24, 24);
    
    // Use subtle, monochrome icons
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(CreateMonochromeIcon("edit", Color.DarkGray));
    imageList.Images.Add(CreateMonochromeIcon("cut", Color.DarkGray));
    imageList.Images.Add(CreateMonochromeIcon("copy", Color.DarkGray));
    imageList.Images.Add(CreateMonochromeIcon("paste", Color.DarkGray));
    
    this.radialMenu1.ImageList = imageList;
    
    // Add items with clean text
    string[] actions = { "Edit", "Cut", "Copy", "Paste" };
    for (int i = 0; i < actions.Length; i++)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = actions[i];
        item.ImageIndex = i;
        this.radialMenu1.Items.Add(item);
    }
    
    // Set form background to complement white theme
    this.ParentForm.BackColor = Color.FromArgb(250, 250, 250);
}

private Image CreateMonochromeIcon(string iconName, Color tint)
{
    // Create or load monochrome icons
    Bitmap icon = new Bitmap(32, 32);
    using (Graphics g = Graphics.FromImage(icon))
    {
        g.Clear(Color.Transparent);
        // Draw simple monochrome icon
        // ... icon drawing code ...
    }
    return icon;
}
```

**When to Use Office2016White Theme:**
- Document editing applications
- Productivity software
- Content-focused applications (writing, reading)
- Professional business applications
- Applications requiring minimal visual distraction

**Best Practices:**
```csharp
// Use on white or very light backgrounds
this.BackColor = Color.White;

// Prefer subtle, monochrome icons
this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;

// Keep text concise and readable
item.Text = "Save";  // Not "Save Document to File"
```

**Result:**
A sophisticated, professional menu that doesn't compete for attention with content.

## Office2016DarkGray Theme

The Office2016DarkGray theme offers a balanced dark appearance that's easier on the eyes than pure black while maintaining professional aesthetics.

### Applying Office2016DarkGray Theme

```csharp
// Apply Office2016DarkGray theme
this.radialMenu1.Style = RadialMenuStyle.Office2016DarkGray;
```

**Visual Characteristics:**
- Medium-dark gray background
- Balanced contrast
- Easy on eyes in various lighting
- Professional appearance
- Versatile for day/night use

**Complete Example:**

```csharp
private void ApplyOffice2016DarkGrayTheme()
{
    // Set Office2016DarkGray theme
    this.radialMenu1.Style = RadialMenuStyle.Office2016DarkGray;
    
    // Configure for dark theme
    this.radialMenu1.Size = new Size(320, 320);
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
    this.radialMenu1.MenuItemImageSize = new Size(28, 28);
    
    // Use light-colored icons for dark background
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(CreateLightIcon("new"));
    imageList.Images.Add(CreateLightIcon("open"));
    imageList.Images.Add(CreateLightIcon("save"));
    imageList.Images.Add(CreateLightIcon("close"));
    
    this.radialMenu1.ImageList = imageList;
    
    // Add items
    string[] actions = { "New", "Open", "Save", "Close" };
    for (int i = 0; i < actions.Length; i++)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = actions[i];
        item.ImageIndex = i;
        this.radialMenu1.Items.Add(item);
    }
    
    // Set dark background for form
    this.ParentForm.BackColor = Color.FromArgb(62, 62, 66);  // VS Code dark gray
}

private Image CreateLightIcon(string name)
{
    // Create icons with light colors for dark backgrounds
    Bitmap icon = new Bitmap(32, 32);
    using (Graphics g = Graphics.FromImage(icon))
    {
        g.Clear(Color.Transparent);
        // Draw light-colored icon elements
        using (Pen lightPen = new Pen(Color.FromArgb(220, 220, 220), 2))
        {
            // ... draw icon with light colors ...
        }
    }
    return icon;
}
```

**When to Use Office2016DarkGray Theme:**
- Code editors and development tools
- Long-session applications (reduces eye strain)
- Media editing applications
- Professional tools used in dim environments
- Applications with dark mode preference

**Best Practices:**
```csharp
// Use light icons on dark background
imageList.Images.Add(CreateIconWithColor(Color.White));

// Ensure text is light-colored
this.radialMenu1.ForeColor = Color.FromArgb(220, 220, 220);

// Pair with dark application backgrounds
this.ParentForm.BackColor = Color.FromArgb(45, 45, 48);

// Consider adding toggle for light/dark themes
toggleDarkMode.Click += (s, e) =>
{
    this.radialMenu1.Style = isDarkMode 
        ? RadialMenuStyle.Office2016DarkGray 
        : RadialMenuStyle.Office2016White;
};
```

**Result:**
A professional dark theme that's comfortable for extended use without being too stark.

## Office2016Black Theme

The Office2016Black theme provides the highest contrast with a true black background, ideal for OLED displays, low-light environments, and users preferring maximum contrast.

### Applying Office2016Black Theme

```csharp
// Apply Office2016Black theme
this.radialMenu1.Style = RadialMenuStyle.Office2016Black;
```

**Visual Characteristics:**
- True black background
- Maximum contrast
- Minimal light emission (OLED-friendly)
- Bold, dramatic appearance
- Excellent for dark environments

**Complete Example:**

```csharp
private void ApplyOffice2016BlackTheme()
{
    // Set Office2016Black theme
    this.radialMenu1.Style = RadialMenuStyle.Office2016Black;
    
    // Optimize for high contrast
    this.radialMenu1.Size = new Size(320, 320);
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
    this.radialMenu1.MenuItemImageSize = new Size(28, 28);
    
    // Use bright, high-contrast icons
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(CreateBrightIcon("new", Color.White));
    imageList.Images.Add(CreateBrightIcon("open", Color.White));
    imageList.Images.Add(CreateBrightIcon("save", Color.White));
    imageList.Images.Add(CreateBrightIcon("exit", Color.White));
    
    this.radialMenu1.ImageList = imageList;
    
    // Add items with clear text
    string[] actions = { "New", "Open", "Save", "Exit" };
    for (int i = 0; i < actions.Length; i++)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = actions[i];
        item.ImageIndex = i;
        this.radialMenu1.Items.Add(item);
    }
    
    // Set pure black background
    this.ParentForm.BackColor = Color.Black;
}

private Image CreateBrightIcon(string name, Color iconColor)
{
    // Create high-contrast white or bright icons
    Bitmap icon = new Bitmap(32, 32);
    using (Graphics g = Graphics.FromImage(icon))
    {
        g.Clear(Color.Transparent);
        g.SmoothingMode = System.Drawing.Drawing2D.SmoothingMode.AntiAlias;
        
        // Draw with bright colors for maximum visibility
        using (Pen brightPen = new Pen(iconColor, 3))
        using (Brush brightBrush = new SolidBrush(iconColor))
        {
            // ... draw high-contrast icon ...
        }
    }
    return icon;
}
```

**When to Use Office2016Black Theme:**
- Night-time usage or dark rooms
- OLED displays (saves battery)
- Video/photo editing applications
- Accessibility - high contrast needs
- Gaming or entertainment applications
- Reducing eye strain in dark environments

**Best Practices:**
```csharp
// Use bright white or colored icons
imageList.Images.Add(CreateIconWithColor(Color.White));

// Ensure maximum text visibility
this.radialMenu1.ForeColor = Color.White;

// Pair with pure black backgrounds
this.ParentForm.BackColor = Color.Black;

// Consider user preference
if (SystemInformation.HighContrast)
{
    this.radialMenu1.Style = RadialMenuStyle.Office2016Black;
}

// Add accent colors for important items
importantItem.ForeColor = Color.FromArgb(0, 200, 255);  // Bright cyan accent
```

**Result:**
A striking, high-contrast menu perfect for low-light environments and maximum visibility.

## Theme Comparison and Selection Guide

### Quick Reference Table

| Theme | Background | Best For | Avoid When |
|-------|-----------|----------|------------|
| **Default** | Light gray | Legacy apps, prototypes | Modern apps needing Office look |
| **Office2016Colorful** | Bright colors | Modern apps, creative tools | Professional/serious contexts |
| **Office2016White** | Pure white | Document editors, productivity | Dark environments |
| **Office2016DarkGray** | Medium dark | Code editors, long sessions | High-brightness needed |
| **Office2016Black** | True black | Night use, OLED, high contrast | Bright environments |

### Choosing the Right Theme

```csharp
private RadialMenuStyle SelectThemeBasedOnContext()
{
    // Check system settings
    if (SystemInformation.HighContrast)
    {
        return RadialMenuStyle.Office2016Black;
    }
    
    // Check time of day (example heuristic)
    int currentHour = DateTime.Now.Hour;
    if (currentHour < 6 || currentHour > 20)
    {
        // Night time - prefer dark themes
        return RadialMenuStyle.Office2016DarkGray;
    }
    
    // Check application type
    if (IsCreativeApplication())
    {
        return RadialMenuStyle.Office2016Colorful;
    }
    else if (IsProductivityApplication())
    {
        return RadialMenuStyle.Office2016White;
    }
    
    // Default fallback
    return RadialMenuStyle.Default;
}

private bool IsCreativeApplication()
{
    // Determine if app is creative/design-focused
    return ApplicationType == AppType.Graphics 
        || ApplicationType == AppType.Video 
        || ApplicationType == AppType.Music;
}

private bool IsProductivityApplication()
{
    // Determine if app is productivity-focused
    return ApplicationType == AppType.TextEditor 
        || ApplicationType == AppType.Spreadsheet 
        || ApplicationType == AppType.Email;
}
```

## Setting Themes at Design Time vs Runtime

### Design Time Configuration

```csharp
// Set in the Properties window or InitializeComponent
private void InitializeComponent()
{
    this.radialMenu1 = new Syncfusion.Windows.Forms.Tools.RadialMenu();
    
    // ... other initialization ...
    
    this.radialMenu1.Style = Syncfusion.Windows.Forms.Tools.RadialMenuStyle.Office2016Colorful;
    
    // ... additional configuration ...
}
```

### Runtime Configuration

```csharp
// Change theme dynamically at runtime
private void ApplyTheme(RadialMenuStyle style)
{
    this.radialMenu1.Style = style;
}

// Example: Theme switcher
private void themeComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    string selectedTheme = themeComboBox.SelectedItem.ToString();
    
    switch (selectedTheme)
    {
        case "Default":
            this.radialMenu1.Style = RadialMenuStyle.Default;
            break;
        case "Colorful":
            this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
            break;
        case "White":
            this.radialMenu1.Style = RadialMenuStyle.Office2016White;
            break;
        case "Dark Gray":
            this.radialMenu1.Style = RadialMenuStyle.Office2016DarkGray;
            break;
        case "Black":
            this.radialMenu1.Style = RadialMenuStyle.Office2016Black;
            break;
    }
    
    // Save preference
    Properties.Settings.Default.MenuTheme = selectedTheme;
    Properties.Settings.Default.Save();
}
```

### Loading Saved Theme Preference

```csharp
private void LoadThemePreference()
{
    // Load theme from settings on application start
    string savedTheme = Properties.Settings.Default.MenuTheme;
    
    if (!string.IsNullOrEmpty(savedTheme))
    {
        RadialMenuStyle style = ParseThemeString(savedTheme);
        this.radialMenu1.Style = style;
    }
}

private RadialMenuStyle ParseThemeString(string themeString)
{
    switch (themeString)
    {
        case "Colorful":
            return RadialMenuStyle.Office2016Colorful;
        case "White":
            return RadialMenuStyle.Office2016White;
        case "Dark Gray":
            return RadialMenuStyle.Office2016DarkGray;
        case "Black":
            return RadialMenuStyle.Office2016Black;
        default:
            return RadialMenuStyle.Default;
    }
}
```

## Theme Best Practices

**1. Consistency Across Application**
```csharp
// Apply same theme to all RadialMenu instances
private void ApplyThemeToAllMenus(RadialMenuStyle style)
{
    mainMenu.Style = style;
    contextMenu.Style = style;
    formatMenu.Style = style;
    toolsMenu.Style = style;
}
```

**2. Match Application Background**
```csharp
// Coordinate menu theme with form background
private void CoordinateThemes()
{
    if (radialMenu1.Style == RadialMenuStyle.Office2016Black)
    {
        this.BackColor = Color.Black;
    }
    else if (radialMenu1.Style == RadialMenuStyle.Office2016White)
    {
        this.BackColor = Color.White;
    }
}
```

**3. Icon Compatibility**
```csharp
// Adjust icons based on theme
private void SetIconsForTheme(RadialMenuStyle style)
{
    ImageListAdv iconSet;
    
    if (style == RadialMenuStyle.Office2016Black || 
        style == RadialMenuStyle.Office2016DarkGray)
    {
        iconSet = lightIconsImageList;  // Light icons for dark themes
    }
    else
    {
        iconSet = darkIconsImageList;   // Dark icons for light themes
    }
    
    this.radialMenu1.ImageList = iconSet;
}
```

**4. User Preference Storage**
```csharp
// Respect and persist user theme choice
private void SaveThemePreference(RadialMenuStyle style)
{
    Properties.Settings.Default.PreferredMenuTheme = style.ToString();
    Properties.Settings.Default.Save();
}

private void RestoreThemePreference()
{
    string savedTheme = Properties.Settings.Default.PreferredMenuTheme;
    if (Enum.TryParse<RadialMenuStyle>(savedTheme, out var style))
    {
        this.radialMenu1.Style = style;
    }
}
```

**5. Accessibility Considerations**
```csharp
// Respect system high contrast settings
private void CheckAccessibilitySettings()
{
    if (SystemInformation.HighContrast)
    {
        // Use highest contrast theme
        this.radialMenu1.Style = RadialMenuStyle.Office2016Black;
    }
}
```

**6. Theme Testing**
```csharp
// Test menu with all themes during development
private void TestAllThemes()
{
    var themes = new[]
    {
        RadialMenuStyle.Default,
        RadialMenuStyle.Office2016Colorful,
        RadialMenuStyle.Office2016White,
        RadialMenuStyle.Office2016DarkGray,
        RadialMenuStyle.Office2016Black
    };
    
    foreach (var theme in themes)
    {
        this.radialMenu1.Style = theme;
        // Verify appearance, readability, icon visibility
        MessageBox.Show($"Testing {theme} theme");
    }
}
```

## Complete Theme Switching Example

```csharp
public class ThemeAwareForm : Form
{
    private RadialMenu mainMenu;
    private ComboBox themeSelector;
    private CheckBox autoThemeCheckBox;

    private void InitializeTheming()
    {
        // Create theme selector
        this.themeSelector = new ComboBox();
        this.themeSelector.Items.AddRange(new object[]
        {
            "Default",
            "Office 2016 Colorful",
            "Office 2016 White",
            "Office 2016 Dark Gray",
            "Office 2016 Black"
        });
        this.themeSelector.SelectedIndexChanged += ThemeSelector_Changed;
        
        // Create auto-theme checkbox
        this.autoThemeCheckBox = new CheckBox();
        this.autoThemeCheckBox.Text = "Auto Theme (Time-based)";
        this.autoThemeCheckBox.CheckedChanged += AutoTheme_Changed;
        
        // Load saved preference
        LoadSavedThemePreference();
    }

    private void ThemeSelector_Changed(object sender, EventArgs e)
    {
        if (autoThemeCheckBox.Checked)
            return;  // Don't change if auto mode is on
        
        ApplySelectedTheme();
    }

    private void ApplySelectedTheme()
    {
        string selection = themeSelector.SelectedItem?.ToString();
        RadialMenuStyle style = RadialMenuStyle.Default;
        
        switch (selection)
        {
            case "Default":
                style = RadialMenuStyle.Default;
                this.BackColor = SystemColors.Control;
                break;
            case "Office 2016 Colorful":
                style = RadialMenuStyle.Office2016Colorful;
                this.BackColor = Color.White;
                break;
            case "Office 2016 White":
                style = RadialMenuStyle.Office2016White;
                this.BackColor = Color.White;
                break;
            case "Office 2016 Dark Gray":
                style = RadialMenuStyle.Office2016DarkGray;
                this.BackColor = Color.FromArgb(62, 62, 66);
                break;
            case "Office 2016 Black":
                style = RadialMenuStyle.Office2016Black;
                this.BackColor = Color.Black;
                break;
        }
        
        mainMenu.Style = style;
        UpdateIconsForTheme(style);
        SaveThemePreference(selection);
    }

    private void AutoTheme_Changed(object sender, EventArgs e)
    {
        if (autoThemeCheckBox.Checked)
        {
            ApplyTimeBasedTheme();
        }
    }

    private void ApplyTimeBasedTheme()
    {
        int hour = DateTime.Now.Hour;
        
        if (hour >= 6 && hour < 18)
        {
            // Daytime: Use light theme
            themeSelector.SelectedItem = "Office 2016 White";
        }
        else
        {
            // Night time: Use dark theme
            themeSelector.SelectedItem = "Office 2016 Dark Gray";
        }
    }

    private void UpdateIconsForTheme(RadialMenuStyle style)
    {
        bool isDarkTheme = style == RadialMenuStyle.Office2016DarkGray ||
                          style == RadialMenuStyle.Office2016Black;
        
        mainMenu.ImageList = isDarkTheme ? lightIconSet : darkIconSet;
    }

    private void SaveThemePreference(string theme)
    {
        Properties.Settings.Default.MenuTheme = theme;
        Properties.Settings.Default.Save();
    }

    private void LoadSavedThemePreference()
    {
        string savedTheme = Properties.Settings.Default.MenuTheme;
        if (!string.IsNullOrEmpty(savedTheme))
        {
            themeSelector.SelectedItem = savedTheme;
        }
        else
        {
            themeSelector.SelectedIndex = 1;  // Default to Colorful
        }
    }
}
```

**Result:**
A fully functional theme system with user preferences, automatic time-based switching, and proper icon coordination.
