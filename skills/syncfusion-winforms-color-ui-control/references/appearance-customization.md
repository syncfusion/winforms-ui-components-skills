# Appearance and Customization

This guide covers visual customization options for ColorUIControl including border styles, panel sizing, and tab text customization.

## Table of Contents
- [Overview](#overview)
- [Border Styles](#border-styles)
- [Panel Sizing and Stretching](#panel-sizing-and-stretching)
- [Tab Text Customization](#tab-text-customization)
- [Font Styling](#font-styling)
- [Complete Customization Examples](#complete-customization-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

ColorUIControl provides several properties to customize its visual appearance:

- **BorderStyle** - Control the border appearance (FixedSingle, Fixed3D, None)
- **Panel Stretching** - Enable dynamic resizing of color panels
- **Tab Names** - Customize the text displayed on each color group tab
- **Font** - Change the font style for tab text

These customization options help integrate ColorUIControl seamlessly into your application's visual design.

## Border Styles

The `BorderStyle` property controls the appearance of the control's border. This affects the overall visual weight and integration with your form's design.

### Available Border Styles

```csharp
public enum BorderStyle
{
    None = 0,        // No border
    FixedSingle = 1, // Single-line border
    Fixed3D = 2      // 3D border (default)
}
```

### Setting Border Style

**C#:**
```csharp
// Fixed single-line border (flat, modern look)
this.colorUIControl1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;

// 3D border (classic Windows look, default)
this.colorUIControl1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;

// No border (seamless integration)
this.colorUIControl1.BorderStyle = System.Windows.Forms.BorderStyle.None;
```

**VB.NET:**
```vb
' Fixed single-line border (flat, modern look)
Me.colorUIControl1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle

' 3D border (classic Windows look, default)
Me.colorUIControl1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D

' No border (seamless integration)
Me.colorUIControl1.BorderStyle = System.Windows.Forms.BorderStyle.None
```

### Border Style Use Cases

**FixedSingle** - Best for:
- Modern, flat UI designs
- Integration with Material Design or Fluent UI
- Minimalist interfaces
- When used inside panels with borders

**Fixed3D** - Best for:
- Traditional Windows applications
- Classic desktop applications
- When you want the control to stand out
- Default Windows Forms aesthetic

**None** - Best for:
- Custom container controls
- When parent container provides the border
- Popup/dropdown scenarios
- Custom-styled applications

### Example: Border Style Based on Theme

**C#:**
```csharp
private void ApplyTheme(string theme)
{
    switch (theme.ToLower())
    {
        case "modern":
            colorUIControl1.BorderStyle = BorderStyle.FixedSingle;
            colorUIControl1.BackColor = Color.White;
            break;
            
        case "classic":
            colorUIControl1.BorderStyle = BorderStyle.Fixed3D;
            colorUIControl1.BackColor = SystemColors.Control;
            break;
            
        case "minimal":
            colorUIControl1.BorderStyle = BorderStyle.None;
            colorUIControl1.BackColor = Color.Transparent;
            break;
    }
}
```

## Panel Sizing and Stretching

ColorUIControl provides properties to enable dynamic stretching of the Custom and User color panels when the control is resized. This creates a more responsive and adaptive user interface.

### CustomColorsStretchOnResize

Enables or disables stretching of the Custom colors panel when the control is resized.

**C#:**
```csharp
// Enable stretching for Custom colors panel
this.colorUIControl1.CustomColorsStretchOnResize = true;

// Disable stretching (fixed size)
this.colorUIControl1.CustomColorsStretchOnResize = false;
```

**VB.NET:**
```vb
' Enable stretching for Custom colors panel
Me.colorUIControl1.CustomColorsStretchOnResize = True

' Disable stretching (fixed size)
Me.colorUIControl1.CustomColorsStretchOnResize = False
```

### UserColorsStretchOnResize

Enables or disables stretching of the User colors panel when the control is resized.

**C#:**
```csharp
// Enable stretching for User colors panel
this.colorUIControl1.UserColorsStretchOnResize = true;

// Disable stretching (fixed size)
this.colorUIControl1.UserColorsStretchOnResize = false;
```

**VB.NET:**
```vb
' Enable stretching for User colors panel
Me.colorUIControl1.UserColorsStretchOnResize = True

' Disable stretching (fixed size)
Me.colorUIControl1.UserColorsStretchOnResize = False
```

### Enabling Both for Consistent Behavior

**C#:**
```csharp
// Enable stretching for both panels
this.colorUIControl1.CustomColorsStretchOnResize = true;
this.colorUIControl1.UserColorsStretchOnResize = true;

// Set a larger size to take advantage of stretching
this.colorUIControl1.Size = new System.Drawing.Size(300, 250);
```

**VB.NET:**
```vb
' Enable stretching for both panels
Me.colorUIControl1.CustomColorsStretchOnResize = True
Me.colorUIControl1.UserColorsStretchOnResize = True

' Set a larger size to take advantage of stretching
Me.colorUIControl1.Size = New System.Drawing.Size(300, 250)
```

### Panel Stretching Benefits

1. **Responsive Design** - Adapts to different screen sizes and resolutions
2. **Better Color Visibility** - Larger color cells are easier to see and click
3. **Flexible Layouts** - Works better in dockable or resizable containers
4. **Improved UX** - More comfortable for touch interfaces

### Example: Responsive Color Picker Panel

**C#:**
```csharp
private void SetupResponsiveColorPicker()
{
    // Create a panel to host the color picker
    Panel container = new Panel();
    container.Dock = DockStyle.Fill;
    container.Padding = new Padding(10);
    
    // Create ColorUIControl
    colorUIControl1 = new ColorUIControl();
    colorUIControl1.Dock = DockStyle.Fill;
    
    // Enable stretching for responsive behavior
    colorUIControl1.CustomColorsStretchOnResize = true;
    colorUIControl1.UserColorsStretchOnResize = true;
    
    // Modern appearance
    colorUIControl1.BorderStyle = BorderStyle.FixedSingle;
    
    // Add to container
    container.Controls.Add(colorUIControl1);
    this.Controls.Add(container);
}
```

## Tab Text Customization

Customize the text displayed on each color group tab to match your application's terminology or localization requirements.

### Tab Name Properties

ColorUIControl provides four properties to customize tab text:

| Property | Default Text | Color Group |
|----------|-------------|-------------|
| `SystemTabName` | "System" | System colors |
| `StandardTabName` | "Standard" | Standard colors |
| `CustomTabName` | "Custom" | Custom colors |
| `UserTabName` | "User" | User colors |

### Setting Custom Tab Names

**C#:**
```csharp
// Customize tab names
this.colorUIControl1.StandardTabName = "Web Colors";
this.colorUIControl1.SystemTabName = "System Colors";
this.colorUIControl1.UserTabName = "My Colors";
this.colorUIControl1.CustomTabName = "Theme Palette";
```

**VB.NET:**
```vb
' Customize tab names
Me.colorUIControl1.StandardTabName = "Web Colors"
Me.colorUIControl1.SystemTabName = "System Colors"
Me.colorUIControl1.UserTabName = "My Colors"
Me.colorUIControl1.CustomTabName = "Theme Palette"
```

### Resetting Tab Names

Each tab name property has a corresponding reset method that restores the default text:

**C#:**
```csharp
// Reset individual tab names to default
this.colorUIControl1.ResetStandardTabName();
this.colorUIControl1.ResetSystemTabName();
this.colorUIControl1.ResetCustomTabName();
this.colorUIControl1.ResetUserTabName();
```

**VB.NET:**
```vb
' Reset individual tab names to default
Me.colorUIControl1.ResetStandardTabName()
Me.colorUIControl1.ResetSystemTabName()
Me.colorUIControl1.ResetCustomTabName()
Me.colorUIControl1.ResetUserTabName()
```

### Tab Name Localization Example

**C#:**
```csharp
private void LocalizeTabNames(string language)
{
    switch (language)
    {
        case "es": // Spanish
            colorUIControl1.SystemTabName = "Sistema";
            colorUIControl1.StandardTabName = "Estándar";
            colorUIControl1.CustomTabName = "Personalizado";
            colorUIControl1.UserTabName = "Usuario";
            break;
            
        case "fr": // French
            colorUIControl1.SystemTabName = "Système";
            colorUIControl1.StandardTabName = "Standard";
            colorUIControl1.CustomTabName = "Personnalisé";
            colorUIControl1.UserTabName = "Utilisateur";
            break;
            
        case "de": // German
            colorUIControl1.SystemTabName = "System";
            colorUIControl1.StandardTabName = "Standard";
            colorUIControl1.CustomTabName = "Benutzerdefiniert";
            colorUIControl1.UserTabName = "Benutzer";
            break;
            
        default: // English
            colorUIControl1.ResetSystemTabName();
            colorUIControl1.ResetStandardTabName();
            colorUIControl1.ResetCustomTabName();
            colorUIControl1.ResetUserTabName();
            break;
    }
}
```

### Context-Specific Tab Names

**C#:**
```csharp
// For a design application
colorUIControl1.StandardTabName = "Basic";
colorUIControl1.CustomTabName = "Palette";
colorUIControl1.UserTabName = "Recent";

// For a branding application
colorUIControl1.StandardTabName = "Web Safe";
colorUIControl1.CustomTabName = "Brand Colors";
colorUIControl1.UserTabName = "Favorites";

// For a theme editor
colorUIControl1.StandardTabName = "Colors";
colorUIControl1.CustomTabName = "Theme";
colorUIControl1.UserTabName = "Custom";
```

## Font Styling

The `Font` property allows you to change the font style for the tab text, affecting the overall appearance and readability.

### Setting Font

**C#:**
```csharp
// Set font for tab text
this.colorUIControl1.Font = new System.Drawing.Font("Segoe UI", 9F);

// Bold font for emphasis
this.colorUIControl1.Font = new System.Drawing.Font("Arial", 9F, FontStyle.Bold);

// Larger font for accessibility
this.colorUIControl1.Font = new System.Drawing.Font("Tahoma", 10F);
```

**VB.NET:**
```vb
' Set font for tab text
Me.colorUIControl1.Font = New System.Drawing.Font("Segoe UI", 9F)

' Bold font for emphasis
Me.colorUIControl1.Font = New System.Drawing.Font("Arial", 9F, FontStyle.Bold)

' Larger font for accessibility
Me.colorUIControl1.Font = New System.Drawing.Font("Tahoma", 10F)
```

### Font Best Practices

1. **Match Application Theme**
   ```csharp
   // Use the same font as your main form
   colorUIControl1.Font = this.Font;
   ```

2. **Ensure Readability**
   ```csharp
   // Don't go smaller than 8pt
   colorUIControl1.Font = new Font("Segoe UI", 8.5F);
   ```

3. **Consider DPI Scaling**
   ```csharp
   // Use system font for automatic DPI scaling
   colorUIControl1.Font = SystemFonts.MessageBoxFont;
   ```

## Complete Customization Examples

### Example 1: Modern Flat Design

**C#:**
```csharp
private void ApplyModernDesign()
{
    // Modern border
    colorUIControl1.BorderStyle = BorderStyle.FixedSingle;
    
    // Clean, modern font
    colorUIControl1.Font = new Font("Segoe UI", 9F);
    
    // Simple tab names
    colorUIControl1.StandardTabName = "Colors";
    colorUIControl1.CustomTabName = "Palette";
    colorUIControl1.UserTabName = "Recent";
    
    // Enable stretching for responsive design
    colorUIControl1.CustomColorsStretchOnResize = true;
    colorUIControl1.UserColorsStretchOnResize = true;
    
    // Clean background
    colorUIControl1.BackColor = Color.White;
    
    // Show only relevant groups
    colorUIControl1.ColorGroups = 
        ColorUIGroups.StandardColors | 
        ColorUIGroups.CustomColors | 
        ColorUIGroups.UserColors;
}
```

### Example 2: Classic Windows Design

**C#:**
```csharp
private void ApplyClassicDesign()
{
    // Classic 3D border
    colorUIControl1.BorderStyle = BorderStyle.Fixed3D;
    
    // Traditional Windows font
    colorUIControl1.Font = new Font("Microsoft Sans Serif", 8.25F);
    
    // Keep default tab names
    colorUIControl1.ResetStandardTabName();
    colorUIControl1.ResetSystemTabName();
    colorUIControl1.ResetCustomTabName();
    colorUIControl1.ResetUserTabName();
    
    // Fixed size panels
    colorUIControl1.CustomColorsStretchOnResize = false;
    colorUIControl1.UserColorsStretchOnResize = false;
    
    // System default background
    colorUIControl1.BackColor = SystemColors.Control;
    
    // Show all groups
    colorUIControl1.ColorGroups = 
        ColorUIGroups.SystemColors | 
        ColorUIGroups.StandardColors | 
        ColorUIGroups.CustomColors | 
        ColorUIGroups.UserColors;
}
```

### Example 3: Minimal Embedded Design

**C#:**
```csharp
private void ApplyMinimalDesign()
{
    // No border for seamless integration
    colorUIControl1.BorderStyle = BorderStyle.None;
    
    // Match parent font
    colorUIControl1.Font = this.Font;
    
    // Minimal tab names
    colorUIControl1.StandardTabName = "Standard";
    colorUIControl1.CustomTabName = "Custom";
    
    // Enable stretching
    colorUIControl1.CustomColorsStretchOnResize = true;
    
    // Transparent background
    colorUIControl1.BackColor = Color.Transparent;
    
    // Show only essential groups
    colorUIControl1.ColorGroups = 
        ColorUIGroups.StandardColors | 
        ColorUIGroups.CustomColors;
}
```

### Example 4: High Contrast Accessibility

**C#:**
```csharp
private void ApplyHighContrastDesign()
{
    // Strong border for definition
    colorUIControl1.BorderStyle = BorderStyle.FixedSingle;
    
    // Larger, bold font for readability
    colorUIControl1.Font = new Font("Tahoma", 10F, FontStyle.Bold);
    
    // Clear, descriptive tab names
    colorUIControl1.SystemTabName = "System Colors";
    colorUIControl1.StandardTabName = "Standard Colors";
    
    // Enable stretching for larger color cells
    colorUIControl1.CustomColorsStretchOnResize = true;
    colorUIControl1.UserColorsStretchOnResize = true;
    
    // High contrast colors
    colorUIControl1.BackColor = SystemColors.Window;
    colorUIControl1.ForeColor = SystemColors.WindowText;
    
    // Include system colors for accessibility
    colorUIControl1.ColorGroups = 
        ColorUIGroups.SystemColors | 
        ColorUIGroups.StandardColors;
    
    // Larger size for accessibility
    colorUIControl1.Size = new Size(280, 240);
}
```

### Example 5: Branded Application Design

**C#:**
```csharp
private void ApplyBrandedDesign()
{
    // Modern flat border
    colorUIControl1.BorderStyle = BorderStyle.FixedSingle;
    
    // Corporate font
    colorUIControl1.Font = new Font("Arial", 9F);
    
    // Brand-specific tab names
    colorUIControl1.StandardTabName = "Web Safe";
    colorUIControl1.CustomTabName = "Brand Palette";
    colorUIControl1.UserTabName = "My Colors";
    
    // Enable responsive behavior
    colorUIControl1.CustomColorsStretchOnResize = true;
    colorUIControl1.UserColorsStretchOnResize = true;
    
    // Brand background color
    colorUIControl1.BackColor = Color.FromArgb(250, 250, 250);
    
    // Show brand-relevant groups
    colorUIControl1.ColorGroups = 
        ColorUIGroups.StandardColors | 
        ColorUIGroups.CustomColors | 
        ColorUIGroups.UserColors;
    
    // Populate CustomColors with brand colors
    PopulateBrandColors();
}

private void PopulateBrandColors()
{
    // Example: Populate with company brand colors
    Color[] brandColors = new Color[]
    {
        Color.FromArgb(0, 120, 212),   // Primary
        Color.FromArgb(255, 185, 0),   // Secondary
        Color.FromArgb(16, 124, 16),   // Success
        // ... more brand colors
    };
    
    for (int i = 0; i < brandColors.Length && i < colorUIControl1.UserColors.Count; i++)
    {
        colorUIControl1.UserColors[i] = brandColors[i];
    }
}
```

## Best Practices

### 1. Match Your Application Theme

Always customize ColorUIControl to match your application's visual design:

```csharp
// Inherit form properties
colorUIControl1.Font = this.Font;
colorUIControl1.BackColor = this.BackColor;
```

### 2. Use Stretching for Responsive Layouts

Enable stretching when the control size may vary:

```csharp
// In dockable or resizable containers
colorUIControl1.CustomColorsStretchOnResize = true;
colorUIControl1.UserColorsStretchOnResize = true;
```

### 3. Choose Appropriate Border Styles

- **Fixed3D**: Traditional Windows apps
- **FixedSingle**: Modern flat design
- **None**: Embedded in custom containers

### 4. Localize Tab Names

For international applications, provide localized tab names:

```csharp
// Load from resource files
colorUIControl1.StandardTabName = Resources.StandardColors;
colorUIControl1.CustomTabName = Resources.CustomColors;
```

### 5. Consider Accessibility

- Use larger fonts (10pt+) for accessibility
- Enable high contrast mode support
- Include System colors group for theme consistency

### 6. Test at Different Sizes

```csharp
// Ensure control works at minimum and maximum sizes
colorUIControl1.MinimumSize = new Size(210, 180);
colorUIControl1.MaximumSize = new Size(400, 350);
```

## Troubleshooting

### Issue: Tab Names Not Changing

**Solution:** Ensure you're setting the property after the control is initialized:
```csharp
// Set after InitializeComponent()
colorUIControl1.StandardTabName = "Web Colors";
```

### Issue: Stretching Not Working

**Solution:** Verify the control has room to stretch:
```csharp
// Control must be resizable
colorUIControl1.Anchor = AnchorStyles.Top | AnchorStyles.Bottom | 
                         AnchorStyles.Left | AnchorStyles.Right;
// Or use Dock
colorUIControl1.Dock = DockStyle.Fill;
```

### Issue: Font Changes Not Visible

**Solution:** Font changes apply to tabs only, not the control content:
```csharp
// This affects tab text
colorUIControl1.Font = new Font("Arial", 10F);
```

### Issue: Border Not Visible

**Solution:** Check background color contrast:
```csharp
// Ensure border is visible against background
if (this.BackColor == Color.White)
{
    colorUIControl1.BorderStyle = BorderStyle.FixedSingle;
}
```

### Issue: Tab Name Reset Not Working

**Solution:** Call the correct reset method:
```csharp
// Use specific reset methods, not setting to empty string
colorUIControl1.ResetStandardTabName(); // Correct
// colorUIControl1.StandardTabName = ""; // Wrong
```

## Next Steps

- [Color Groups](color-groups.md) - Learn about configuring color groups
- [Events](events.md) - Handle color selection events
- [Popup Integration](popup-integration.md) - Use ColorUIControl in popup menus
