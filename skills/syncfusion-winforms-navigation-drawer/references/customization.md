# Customization and Styling

This guide covers visual customization options for the Navigation Drawer, including built-in themes and custom color configurations.

## Built-in Themes

The Navigation Drawer control includes five built-in Office 2016-inspired themes for professional appearance. Set the theme using the `Style` property.

### Default Theme

The default theme provides a clean, neutral appearance.

```csharp
this.navigationDrawer1.Style = NavigationDrawerStyle.Default;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Style = NavigationDrawerStyle.Default
```

![Default theme](../assets/Style_img1.png)

**Characteristics:**
- Neutral gray color scheme
- Standard borders and spacing
- Suitable for any application type
- Good contrast for readability

### Office2016Colorful Theme

A vibrant theme with blue accents inspired by Office 2016.

```csharp
this.navigationDrawer1.Style = NavigationDrawerStyle.Office2016Colorful;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Style = NavigationDrawerStyle.Office2016Colorful
```

![Office2016Colorful theme](../assets/Style_img2.png)

**Characteristics:**
- Blue accent colors
- Modern, professional appearance
- High visual impact
- Best for business applications

### Office2016White Theme

A clean, light theme with minimal color accents.

```csharp
this.navigationDrawer1.Style = NavigationDrawerStyle.Office2016White;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Style = NavigationDrawerStyle.Office2016White
```

![Office2016White theme](../assets/Style_img3.png)

**Characteristics:**
- Predominantly white background
- Subtle gray accents
- Minimal visual weight
- Best for content-focused applications

### Office2016DarkGray Theme

A medium-dark theme that reduces eye strain.

```csharp
this.navigationDrawer1.Style = NavigationDrawerStyle.Office2016DarkGray;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Style = NavigationDrawerStyle.Office2016DarkGray
```

![Office2016DarkGray theme](../assets/Style_img4.png)

**Characteristics:**
- Dark gray background
- Light text for contrast
- Reduced brightness
- Best for extended use or low-light environments

### Office2016Black Theme

A high-contrast dark theme.

```csharp
this.navigationDrawer1.Style = NavigationDrawerStyle.Office2016Black;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Style = NavigationDrawerStyle.Office2016Black
```

![Office2016Black theme](../assets/Style_img5.png)

**Characteristics:**
- Black background
- High contrast with white text
- Modern, sleek appearance
- Best for design-focused or media applications

### Theme Selection Guidelines

| Theme | Best For | Use When |
|-------|----------|----------|
| Default | General purpose | Unsure which theme to use |
| Office2016Colorful | Business apps | Want vibrant, professional look |
| Office2016White | Content-focused | Minimalist design needed |
| Office2016DarkGray | Long sessions | Reducing eye strain |
| Office2016Black | Media/Design | High contrast desired |

## Custom Color Customization

Beyond built-in themes, you can customize individual menu item colors.

### DefaultColor Property

The `DefaultColor` property sets the base background color of a menu item.

```csharp
// Set default color for menu item
this.drawerMenuItem1.DefaultColor = System.Drawing.Color.LightBlue;
```

**VB.NET:**
```vb
Me.drawerMenuItem1.DefaultColor = System.Drawing.Color.LightBlue
```

**Note:** The `BackColor` will be automatically updated based on the `DefaultColor` value.

### BackColor Property

Set the background color directly:

```csharp
// Set background color
this.drawerMenuItem1.BackColor = System.Drawing.Color.Silver;
```

**VB.NET:**
```vb
Me.drawerMenuItem1.BackColor = System.Drawing.Color.Silver
```

### HoverColor Property

Define the color when the mouse hovers over the menu item:

```csharp
// Set hover color
this.drawerMenuItem1.HoverColor = System.Drawing.Color.White;
```

**VB.NET:**
```vb
Me.drawerMenuItem1.HoverColor = System.Drawing.Color.White
```

### Complete Color Customization Example

```csharp
// Customize all color properties
this.drawerMenuItem1.BackColor = System.Drawing.Color.Silver;
this.drawerMenuItem1.HoverColor = System.Drawing.Color.White;
this.drawerMenuItem1.DefaultColor = System.Drawing.Color.Silver;
```

**VB.NET:**
```vb
Me.drawerMenuItem1.BackColor = System.Drawing.Color.Silver
Me.drawerMenuItem1.HoverColor = System.Drawing.Color.White
Me.drawerMenuItem1.DefaultColor = System.Drawing.Color.Silver
```

![Color customization](../assets/navigationdrawer_img9.png)

## Advanced Color Patterns

### Color-Coded Menu Items

Assign different colors to different menu categories:

```csharp
// Create color-coded navigation
DrawerMenuItem homeItem = new DrawerMenuItem { Text = "Home" };
homeItem.DefaultColor = Color.LightBlue;
homeItem.HoverColor = Color.SkyBlue;

DrawerMenuItem workItem = new DrawerMenuItem { Text = "Work" };
workItem.DefaultColor = Color.LightGreen;
workItem.HoverColor = Color.LimeGreen;

DrawerMenuItem settingsItem = new DrawerMenuItem { Text = "Settings" };
settingsItem.DefaultColor = Color.LightGray;
settingsItem.HoverColor = Color.Silver;

navigationDrawer1.Items.Add(homeItem);
navigationDrawer1.Items.Add(workItem);
navigationDrawer1.Items.Add(settingsItem);
```

### Selected Item Highlighting

Highlight the currently selected menu item:

```csharp
private DrawerMenuItem currentlySelected = null;

private void HighlightMenuItem(DrawerMenuItem item)
{
    // Reset previously selected item
    if (currentlySelected != null)
    {
        currentlySelected.BackColor = currentlySelected.DefaultColor;
    }
    
    // Highlight new selection
    item.BackColor = Color.LightSteelBlue;
    currentlySelected = item;
}
```

### Gradient Effect Simulation

Create a gradient-like effect by varying item colors:

```csharp
// Create a color gradient effect across menu items
Color[] gradientColors = new Color[]
{
    Color.FromArgb(240, 248, 255), // AliceBlue
    Color.FromArgb(230, 240, 255),
    Color.FromArgb(220, 230, 255),
    Color.FromArgb(210, 220, 255),
    Color.FromArgb(200, 210, 255)
};

for (int i = 0; i < menuItems.Count && i < gradientColors.Length; i++)
{
    menuItems[i].DefaultColor = gradientColors[i];
    menuItems[i].BackColor = gradientColors[i];
}
```

## ForeColor Customization

Customize text colors for better contrast and readability.

### Setting Text Color

```csharp
// Set menu item text color
this.drawerMenuItem1.ForeColor = Color.DarkBlue;
```

### Contrast-Aware Text Colors

```csharp
// Ensure good contrast based on background
private void SetContrastingTextColor(DrawerMenuItem item, Color backgroundColor)
{
    // Calculate brightness
    double brightness = (0.299 * backgroundColor.R + 
                        0.587 * backgroundColor.G + 
                        0.114 * backgroundColor.B) / 255;
    
    // Use white text for dark backgrounds, black for light
    item.ForeColor = brightness > 0.5 ? Color.Black : Color.White;
}

// Usage
drawerMenuItem1.BackColor = Color.DarkSlateBlue;
SetContrastingTextColor(drawerMenuItem1, Color.DarkSlateBlue);
```

## Theme Switching at Runtime

Allow users to switch themes dynamically.

### Basic Theme Switcher

```csharp
// Add a settings menu item for theme selection
private void ShowThemeSelector()
{
    var themes = new[]
    {
        NavigationDrawerStyle.Default,
        NavigationDrawerStyle.Office2016Colorful,
        NavigationDrawerStyle.Office2016White,
        NavigationDrawerStyle.Office2016DarkGray,
        NavigationDrawerStyle.Office2016Black
    };
    
    using (var dialog = new Form())
    {
        dialog.Text = "Select Theme";
        dialog.Size = new Size(300, 250);
        
        var listBox = new ListBox();
        listBox.Dock = DockStyle.Fill;
        listBox.Items.AddRange(Enum.GetNames(typeof(NavigationDrawerStyle)));
        
        listBox.DoubleClick += (s, e) =>
        {
            if (listBox.SelectedIndex >= 0)
            {
                navigationDrawer1.Style = themes[listBox.SelectedIndex];
                dialog.Close();
            }
        };
        
        dialog.Controls.Add(listBox);
        dialog.ShowDialog();
    }
}
```

### Theme Persistence

```csharp
// Save theme preference
private void SaveThemePreference(NavigationDrawerStyle style)
{
    Properties.Settings.Default.DrawerTheme = style.ToString();
    Properties.Settings.Default.Save();
}

// Load theme preference
private void LoadThemePreference()
{
    string savedTheme = Properties.Settings.Default.DrawerTheme;
    if (!string.IsNullOrEmpty(savedTheme))
    {
        if (Enum.TryParse<NavigationDrawerStyle>(savedTheme, out var style))
        {
            navigationDrawer1.Style = style;
        }
    }
}

// Call on form load
private void MainForm_Load(object sender, EventArgs e)
{
    LoadThemePreference();
}
```

### System Theme Detection

```csharp
// Detect Windows dark mode and apply appropriate theme
private void ApplySystemTheme()
{
    try
    {
        using (var key = Microsoft.Win32.Registry.CurrentUser.OpenSubKey(
            @"Software\Microsoft\Windows\CurrentVersion\Themes\Personalize"))
        {
            var value = key?.GetValue("AppsUseLightTheme");
            bool isLightTheme = value != null && (int)value == 1;
            
            navigationDrawer1.Style = isLightTheme
                ? NavigationDrawerStyle.Office2016White
                : NavigationDrawerStyle.Office2016DarkGray;
        }
    }
    catch
    {
        // Fallback to default theme
        navigationDrawer1.Style = NavigationDrawerStyle.Default;
    }
}
```

## Header Customization

Customize the drawer header appearance.

### Header Colors

```csharp
DrawerHeader header = new DrawerHeader();
header.Text = "Navigation";
header.BackColor = Color.DarkSlateBlue;
header.ForeColor = Color.White;
header.Font = new Font("Segoe UI", 12, FontStyle.Bold);

navigationDrawer1.Items.Add(header);
```

### Header with Logo

```csharp
DrawerHeader header = new DrawerHeader();
header.Text = "My Application";
header.Image = Properties.Resources.AppLogo;
header.ImageAlign = ContentAlignment.MiddleLeft;
header.TextAlign = ContentAlignment.MiddleCenter;
header.Height = 80;

navigationDrawer1.Items.Add(header);
```

## Best Practices

### Consistent Theming

```csharp
// Apply theme consistently to all drawer elements
private void ApplyConsistentTheme(NavigationDrawerStyle style)
{
    navigationDrawer1.Style = style;
    
    // Ensure all items respect the theme
    foreach (var item in navigationDrawer1.Items)
    {
        if (item is DrawerMenuItem menuItem)
        {
            // Let the theme control colors unless specifically overridden
            // Only customize if you have a specific reason
        }
    }
}
```

### Accessibility Compliance

```csharp
// Ensure sufficient contrast for WCAG compliance
private bool MeetsContrastRequirements(Color foreground, Color background)
{
    double luminance1 = GetRelativeLuminance(foreground);
    double luminance2 = GetRelativeLuminance(background);
    
    double lighter = Math.Max(luminance1, luminance2);
    double darker = Math.Min(luminance1, luminance2);
    
    double contrastRatio = (lighter + 0.05) / (darker + 0.05);
    
    // WCAG AA requires 4.5:1 for normal text
    return contrastRatio >= 4.5;
}

private double GetRelativeLuminance(Color color)
{
    double r = color.R / 255.0;
    double g = color.G / 255.0;
    double b = color.B / 255.0;
    
    r = r <= 0.03928 ? r / 12.92 : Math.Pow((r + 0.055) / 1.055, 2.4);
    g = g <= 0.03928 ? g / 12.92 : Math.Pow((g + 0.055) / 1.055, 2.4);
    b = b <= 0.03928 ? b / 12.92 : Math.Pow((b + 0.055) / 1.055, 2.4);
    
    return 0.2126 * r + 0.7152 * g + 0.0722 * b;
}
```

### Performance Considerations

```csharp
// Avoid excessive color changes during animations
navigationDrawer1.Opening += (sender, e) =>
{
    // Disable visual updates during transition
    navigationDrawer1.SuspendLayout();
};

navigationDrawer1.Opened += (sender, e) =>
{
    // Re-enable updates after transition
    navigationDrawer1.ResumeLayout();
};
```

## Complete Customization Example

```csharp
private void CustomizeNavigationDrawer()
{
    // Apply base theme
    navigationDrawer1.Style = NavigationDrawerStyle.Office2016Colorful;
    
    // Customize header
    DrawerHeader header = new DrawerHeader();
    header.Text = "My App";
    header.BackColor = Color.DarkSlateBlue;
    header.ForeColor = Color.White;
    header.Font = new Font("Segoe UI", 14, FontStyle.Bold);
    header.Height = 60;
    navigationDrawer1.Items.Add(header);
    
    // Create color-coded menu items
    var menuItemsConfig = new[]
    {
        new { Text = "Dashboard", Color = Color.FromArgb(100, 149, 237) },
        new { Text = "Reports", Color = Color.FromArgb(60, 179, 113) },
        new { Text = "Analytics", Color = Color.FromArgb(255, 165, 0) },
        new { Text = "Settings", Color = Color.FromArgb(128, 128, 128) }
    };
    
    foreach (var config in menuItemsConfig)
    {
        DrawerMenuItem item = new DrawerMenuItem();
        item.Text = config.Text;
        item.DefaultColor = config.Color;
        item.BackColor = config.Color;
        item.HoverColor = ControlPaint.Light(config.Color);
        item.ForeColor = Color.White;
        navigationDrawer1.Items.Add(item);
    }
}
```

## Next Steps

- **Handle events:** See [events.md](events.md) for drawer event handling
- **Advanced scenarios:** See [advanced-usage.md](advanced-usage.md) for complex configurations
