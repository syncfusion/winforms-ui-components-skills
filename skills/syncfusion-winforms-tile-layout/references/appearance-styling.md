# Appearance and Styling in TileLayout

This guide covers visual customization options for TileLayout, including flat form appearance, group title display, background colors, and theme integration.

## Overview

TileLayout provides several appearance properties to customize the visual presentation:

- **SetParentFormFlat:** Apply flat, modern look to the parent form
- **ShowGroupTitle:** Display or hide LayoutGroup title text
- **IgnoreThemeBackground:** Use custom BackColor instead of theme colors
- **Theme Integration:** Apply Office, Metro, or custom themes

## SetParentFormFlat

The **SetParentFormFlat** property gives a flat, modern appearance to the parent form when enabled.

**Property:** `tileLayout1.SetParentFormFlat`  
**Type:** `bool`  
**Default:** `false`

```csharp
// Enable flat look for parent form
tileLayout1.SetParentFormFlat = true;
```

```vb
' Enable flat look for parent form
tileLayout1.SetParentFormFlat = True
```

![Flat Parent Form](images/ParentFormFlat.png)

**Effect:**
- Removes 3D borders and shadows from parent form
- Creates modern, flat appearance
- Applies Metro/modern design aesthetic
- Affects form chrome and borders

**When to use:**
- Modern, minimalist UI design
- Windows 8/10 Metro-style applications
- Flat design aesthetics
- Touch-friendly interfaces

**Example - Complete flat form setup:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Create TileLayout with flat parent form
    TileLayout tileLayout1 = new TileLayout();
    tileLayout1.Dock = DockStyle.Fill;
    tileLayout1.SetParentFormFlat = true;
    
    this.Controls.Add(tileLayout1);
    this.FormBorderStyle = FormBorderStyle.None; // Optional: borderless
}
```

## ShowGroupTitle

The **ShowGroupTitle** property controls whether LayoutGroup title text is displayed.

**Property:** `tileLayout1.ShowGroupTitle`  
**Type:** `bool`  
**Default:** `false`

```csharp
// Show group titles
tileLayout1.ShowGroupTitle = true;
```

```vb
' Show group titles
tileLayout1.ShowGroupTitle = True
```

![Group Titles Shown](images/LayoutTitle.png)

**Effect:**
- Displays the `Text` property of each LayoutGroup as a header
- Appears above each tile group
- Helps users understand tile organization
- Provides visual separation between groups

**When to use:**
- Multiple tile groups needing identification
- Categorical organization (e.g., "Apps", "Media", "Tools")
- User navigation assistance
- Dashboard sections with labels

**Example - Groups with titles:**
```csharp
tileLayout1.ShowGroupTitle = true;

// Create groups with descriptive titles
LayoutGroup productivityGroup = new LayoutGroup();
productivityGroup.Text = "Productivity Apps";

LayoutGroup entertainmentGroup = new LayoutGroup();
entertainmentGroup.Text = "Entertainment";

tileLayout1.Controls.Add(productivityGroup);
tileLayout1.Controls.Add(entertainmentGroup);
```

**Hiding titles:**
```csharp
// Hide group titles for cleaner look
tileLayout1.ShowGroupTitle = false;
```

**Use case for hiding:** Minimalist designs, single-group layouts, when groups are self-explanatory.

## IgnoreThemeBackground

The **IgnoreThemeBackground** property determines whether the control uses theme background colors or custom BackColor.

**Property:** `tileLayout1.IgnoreThemeBackground`  
**Type:** `bool`  
**Default:** `false`

**Important:** You **must** set `IgnoreThemeBackground = true` before the TileLayout's BackColor will be applied.

```csharp
// Enable custom BackColor
tileLayout1.IgnoreThemeBackground = true;
tileLayout1.BackColor = Color.LightSteelBlue;
```

```vb
' Enable custom BackColor
tileLayout1.IgnoreThemeBackground = True
tileLayout1.BackColor = Color.LightSteelBlue
```

![Custom Background Color](images/ThemedBackground.png)

**Behavior:**

**When `false` (default):**
- TileLayout uses theme's background color
- BackColor property is ignored
- Visual consistency with active theme

**When `true`:**
- Theme background is ignored
- TileLayout.BackColor is applied
- Custom color schemes possible

**When to use:**
- Custom branding colors required
- Specific color schemes (dark mode, custom themes)
- Background images or gradients (via BackgroundImage)
- Non-standard visual designs

**Example - Custom color scheme:**
```csharp
// Dark theme with custom colors
tileLayout1.IgnoreThemeBackground = true;
tileLayout1.BackColor = Color.FromArgb(32, 32, 32); // Dark gray

// Custom group colors
LayoutGroup group1 = new LayoutGroup();
group1.BackColor = Color.FromArgb(0, 120, 215); // Blue accent
group1.Text = "Primary";

LayoutGroup group2 = new LayoutGroup();
group2.BackColor = Color.FromArgb(16, 124, 16); // Green accent
group2.Text = "Secondary";
```

## Theme Integration

TileLayout supports Syncfusion's theme framework for consistent visual styling.

### Applying Themes

Use Syncfusion's `SkinManager` or theme properties:

```csharp
// Apply Office2016 theme
tileLayout1.ThemeName = "Office2016Colorful";
```

**Available themes:**
- Office2016Colorful
- Office2016White
- Office2016DarkGray
- Office2016Black
- Metro
- Office2019Colorful
- HighContrastBlack

### Theme with Custom Accents

Combine themes with custom colors:

```csharp
// Use theme for TileLayout, custom colors for groups
tileLayout1.ThemeName = "Office2016Colorful";
tileLayout1.IgnoreThemeBackground = false; // Use theme background

// Custom group colors still work
layoutGroup1.BackColor = Color.FromArgb(0, 120, 215);
layoutGroup2.BackColor = Color.FromArgb(232, 17, 35);
```

**Pattern:** Let TileLayout use theme background, but customize LayoutGroup colors for visual distinction.

## Complete Styled Example

Here's a comprehensive example with full appearance customization:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class StyledTileLayoutForm : Form
{
    private TileLayout tileLayout1;
    
    public StyledTileLayoutForm()
    {
        SetupStyledLayout();
    }
    
    private void SetupStyledLayout()
    {
        // Create TileLayout with custom appearance
        tileLayout1 = new TileLayout();
        tileLayout1.Dock = DockStyle.Fill;
        
        // Appearance settings
        tileLayout1.SetParentFormFlat = true;
        tileLayout1.ShowGroupTitle = true;
        tileLayout1.IgnoreThemeBackground = true;
        tileLayout1.BackColor = Color.FromArgb(240, 240, 240); // Light gray
        
        // Layout customization
        tileLayout1.MainLayout.Alignment = FlowAlignment.Center;
        tileLayout1.MainLayout.TopMargin = 30;
        tileLayout1.MainLayout.HorzNearMargin = 40;
        tileLayout1.MainLayout.HorzFarMargin = 40;
        
        // Create styled groups
        CreateStyledGroups();
        
        // Add to form
        this.Controls.Add(tileLayout1);
        this.Text = "Styled TileLayout";
        this.Size = new Size(1000, 700);
        this.BackColor = Color.White;
    }
    
    private void CreateStyledGroups()
    {
        // Modern blue group
        LayoutGroup blueGroup = new LayoutGroup();
        blueGroup.Text = "Primary Tools";
        blueGroup.BackColor = Color.FromArgb(0, 120, 215);
        blueGroup.ForeColor = Color.White;
        AddTilesToGroup(blueGroup, 6, Color.White);
        
        // Modern green group
        LayoutGroup greenGroup = new LayoutGroup();
        greenGroup.Text = "Secondary Tools";
        greenGroup.BackColor = Color.FromArgb(16, 124, 16);
        greenGroup.ForeColor = Color.White;
        AddTilesToGroup(greenGroup, 4, Color.White);
        
        // Modern orange group
        LayoutGroup orangeGroup = new LayoutGroup();
        orangeGroup.Text = "Utilities";
        orangeGroup.BackColor = Color.FromArgb(255, 140, 0);
        orangeGroup.ForeColor = Color.White;
        AddTilesToGroup(orangeGroup, 3, Color.White);
        
        tileLayout1.Controls.Add(blueGroup);
        tileLayout1.Controls.Add(greenGroup);
        tileLayout1.Controls.Add(orangeGroup);
    }
    
    private void AddTilesToGroup(LayoutGroup group, int count, Color tileColor)
    {
        for (int i = 1; i <= count; i++)
        {
            ImageStreamer tile = new ImageStreamer();
            tile.Images.Add(CreateStyledTileImage($"Item {i}"));
            tile.InternalBackColor = tileColor;
            group.Controls.Add(tile);
        }
    }
    
    private Image CreateStyledTileImage(string text)
    {
        Bitmap bmp = new Bitmap(150, 150);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.SmoothingMode = System.Drawing.Drawing2D.SmoothingMode.AntiAlias;
            g.Clear(Color.White);
            
            // Draw icon placeholder
            g.FillEllipse(new SolidBrush(Color.FromArgb(0, 120, 215)), 
                50, 30, 50, 50);
            
            // Draw text
            StringFormat sf = new StringFormat();
            sf.Alignment = StringAlignment.Center;
            g.DrawString(text, new Font("Segoe UI", 10, FontStyle.Bold), 
                Brushes.DarkGray, new RectangleF(0, 100, 150, 40), sf);
        }
        return bmp;
    }
}
```

**Result:** Modern, flat appearance with custom colors, centered layout, and color-coded groups with titles.

## Styling Best Practices

### Color Selection

1. **High Contrast:** Ensure text is readable on group backgrounds
2. **Consistent Palette:** Use related colors across groups (e.g., blue shades)
3. **Brand Colors:** Match corporate/application branding
4. **Accessibility:** Maintain WCAG AA contrast ratios (4.5:1 minimum)

**Example - Accessible color palette:**
```csharp
// Modern blue palette with good contrast
Color primaryBlue = Color.FromArgb(0, 120, 215);   // Bright blue
Color secondaryBlue = Color.FromArgb(0, 99, 177);  // Medium blue
Color accentBlue = Color.FromArgb(0, 78, 138);     // Dark blue
Color textColor = Color.White;                      // White text on blue
```

### Theme Consistency

**Option 1:** Use built-in themes
```csharp
tileLayout1.ThemeName = "Office2016Colorful";
layoutGroup1.BackColor = SystemColors.Control; // Theme-aware
```

**Option 2:** Custom colors throughout
```csharp
tileLayout1.IgnoreThemeBackground = true;
// Apply full custom color scheme
```

**Avoid:** Mixing theme and custom colors inconsistently—choose one approach.

### Visual Hierarchy

Use appearance properties to create clear visual hierarchy:

```csharp
// Main container - light background
tileLayout1.IgnoreThemeBackground = true;
tileLayout1.BackColor = Color.FromArgb(245, 245, 245);

// Groups - medium colored backgrounds
layoutGroup1.BackColor = Color.FromArgb(0, 120, 215);

// Tiles - light/white backgrounds for contrast
imageStreamer1.InternalBackColor = Color.White;
```

### Modern Flat Design

For contemporary Metro/Material design:

```csharp
// Flat parent form
tileLayout1.SetParentFormFlat = true;

// Minimal borders
this.FormBorderStyle = FormBorderStyle.FixedSingle;

// Flat colors (no gradients)
layoutGroup1.BackColor = Color.FromArgb(0, 120, 215);

// Clean spacing
tileLayout1.MainLayout.TopMargin = 20;
```

## Troubleshooting

**Issue:** BackColor not applying
- **Solution:** Set `IgnoreThemeBackground = true` before setting BackColor

**Issue:** Group titles not showing
- **Solution:** Set `ShowGroupTitle = true` and ensure LayoutGroup.Text is set

**Issue:** Parent form still has 3D appearance
- **Solution:** Verify `SetParentFormFlat = true` and check FormBorderStyle

**Issue:** Colors look washed out
- **Solution:** Use saturated colors (RGB values with at least one component >200)

**Issue:** Text illegible on colored backgrounds
- **Solution:** Set group ForeColor explicitly: `layoutGroup1.ForeColor = Color.White`

## Summary

TileLayout appearance customization provides:

- **SetParentFormFlat:** Modern flat form appearance
- **ShowGroupTitle:** Group name display control
- **IgnoreThemeBackground:** Custom color schemes
- **Theme Integration:** Built-in theme support

Combine these properties to create visually appealing, brand-consistent tile layouts that match your application's design requirements.
