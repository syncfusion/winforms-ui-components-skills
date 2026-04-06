# Appearance and Styling

## Table of Contents
- [Overview](#overview)
- [ScrollBar Appearance Customization](#scrollbar-appearance-customization)
  - [Horizontal ScrollBar Styling](#horizontal-scrollbar-styling)
  - [Vertical ScrollBar Styling](#vertical-scrollbar-styling)
- [Thumb Customization](#thumb-customization)
- [Arrow Button Customization](#arrow-button-customization)
- [Disabling Scrollbar Elements](#disabling-scrollbar-elements)
  - [Disabling Arrow Buttons](#disabling-arrow-buttons)
  - [Disabling the Thumb](#disabling-the-thumb)
- [Built-in Themes](#built-in-themes)
  - [Loading Theme Assemblies](#loading-theme-assemblies)
  - [Applying Themes](#applying-themes)
  - [Available Themes](#available-themes)

## Overview

The SfScrollFrame provides extensive appearance customization through the `Style` property of each scrollbar. By default, scrollbars load with standard appearance, but every visual element can be customized including colors, sizes, and states (normal, hover, pressed, disabled).

The `ScrollBarStyleInfo` class contains all settings that control scrollbar appearance, applied separately to horizontal and vertical scrollbars via:
- `sfScrollFrame.HorizontalScrollBar.Style`
- `sfScrollFrame.VerticalScrollBar.Style`

## ScrollBar Appearance Customization

### Horizontal ScrollBar Styling

Customize the horizontal scrollbar using `HorizontalScrollBar.Style` property:

```csharp
using Syncfusion.WinForms.Controls;
using System.Drawing;

// Arrow button back colors
this.sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonBackColor = Color.Gray;
this.sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonHoverBackColor = Color.White;
this.sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonPressedBackColor = Color.Blue;

// Arrow button fore colors (arrow icons)
this.sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonForeColor = Color.Black;
this.sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonHoverForeColor = Color.Black;
this.sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonPressedForeColor = Color.Gray;
this.sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonBorderColor = Color.Black;

// Thumb styling
this.sfScrollFrame1.HorizontalScrollBar.Style.ThumbColor = Color.Gray;
this.sfScrollFrame1.HorizontalScrollBar.Style.ThumbHoverColor = Color.Black;
this.sfScrollFrame1.HorizontalScrollBar.Style.ThumbPressedColor = Color.Blue;
this.sfScrollFrame1.HorizontalScrollBar.Style.ThumbBorderColor = Color.Black;

// Scrollbar track background
this.sfScrollFrame1.HorizontalScrollBar.Style.ScrollBarBackColor = Color.LightGray;
```

### Vertical ScrollBar Styling

Customize the vertical scrollbar using `VerticalScrollBar.Style` property:

```csharp
// Arrow button back colors
this.sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonBackColor = Color.Gray;
this.sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonHoverBackColor = Color.White;
this.sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonPressedBackColor = Color.Blue;

// Arrow button fore colors (arrow icons)
this.sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonForeColor = Color.Black;
this.sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonHoverForeColor = Color.Black;
this.sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonPressedForeColor = Color.Gray;
this.sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonBorderColor = Color.Black;

// Thumb styling
this.sfScrollFrame1.VerticalScrollBar.Style.ThumbColor = Color.Gray;
this.sfScrollFrame1.VerticalScrollBar.Style.ThumbHoverColor = Color.Black;
this.sfScrollFrame1.VerticalScrollBar.Style.ThumbPressedColor = Color.Blue;
this.sfScrollFrame1.VerticalScrollBar.Style.ThumbBorderColor = Color.Black;

// Scrollbar track background
this.sfScrollFrame1.VerticalScrollBar.Style.ScrollBarBackColor = Color.LightGray;
```

### Complete Custom Styling Example

```csharp
private void CustomizeScrollBars()
{
    // Apply custom blue/gray theme to both scrollbars
    
    // === Vertical ScrollBar ===
    // Track
    sfScrollFrame1.VerticalScrollBar.Style.ScrollBarBackColor = Color.FromArgb(240, 240, 240);
    
    // Thumb
    sfScrollFrame1.VerticalScrollBar.Style.ThumbColor = Color.FromArgb(100, 100, 100);
    sfScrollFrame1.VerticalScrollBar.Style.ThumbHoverColor = Color.FromArgb(50, 120, 200);
    sfScrollFrame1.VerticalScrollBar.Style.ThumbPressedColor = Color.FromArgb(30, 90, 160);
    sfScrollFrame1.VerticalScrollBar.Style.ThumbBorderColor = Color.FromArgb(80, 80, 80);
    
    // Arrow Buttons
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonBackColor = Color.FromArgb(200, 200, 200);
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonHoverBackColor = Color.FromArgb(220, 220, 220);
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonPressedBackColor = Color.FromArgb(50, 120, 200);
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonForeColor = Color.Black;
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonHoverForeColor = Color.Black;
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonPressedForeColor = Color.White;
    
    // === Horizontal ScrollBar ===
    // Track
    sfScrollFrame1.HorizontalScrollBar.Style.ScrollBarBackColor = Color.FromArgb(240, 240, 240);
    
    // Thumb
    sfScrollFrame1.HorizontalScrollBar.Style.ThumbColor = Color.FromArgb(100, 100, 100);
    sfScrollFrame1.HorizontalScrollBar.Style.ThumbHoverColor = Color.FromArgb(50, 120, 200);
    sfScrollFrame1.HorizontalScrollBar.Style.ThumbPressedColor = Color.FromArgb(30, 90, 160);
    sfScrollFrame1.HorizontalScrollBar.Style.ThumbBorderColor = Color.FromArgb(80, 80, 80);
    
    // Arrow Buttons
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonBackColor = Color.FromArgb(200, 200, 200);
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonHoverBackColor = Color.FromArgb(220, 220, 220);
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonPressedBackColor = Color.FromArgb(50, 120, 200);
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonForeColor = Color.Black;
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonHoverForeColor = Color.Black;
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonPressedForeColor = Color.White;
}
```

## Thumb Customization

### Changing Thumb Width

Control the size of the scrollbar thumb using the `ThumbWidth` property:

```csharp
// Set horizontal scrollbar thumb height
this.sfScrollFrame1.HorizontalScrollBar.Style.ThumbWidth = 8;

// Set vertical scrollbar thumb width
this.sfScrollFrame1.VerticalScrollBar.Style.ThumbWidth = 8;
```

**Important Notes:**
- `ThumbWidth` refers to the cross-dimension (width for vertical, height for horizontal)
- Maximum value: the width/height of the scrollbar itself
- Minimum value: typically 4-6 pixels for usability
- Smaller thumbs create a more modern, minimalist appearance

### Thumb Width Examples

```csharp
// Thin modern scrollbars (macOS style)
sfScrollFrame1.VerticalScrollBar.Style.ThumbWidth = 6;
sfScrollFrame1.HorizontalScrollBar.Style.ThumbWidth = 6;

// Standard scrollbars
sfScrollFrame1.VerticalScrollBar.Style.ThumbWidth = 12;
sfScrollFrame1.HorizontalScrollBar.Style.ThumbWidth = 12;

// Thick scrollbars (easier to grab)
sfScrollFrame1.VerticalScrollBar.Style.ThumbWidth = 16;
sfScrollFrame1.HorizontalScrollBar.Style.ThumbWidth = 16;
```

### Thumb State Colors

The thumb supports four visual states:

```csharp
// Normal state (default appearance)
sfScrollFrame1.VerticalScrollBar.Style.ThumbColor = Color.Gray;

// Hover state (mouse over thumb)
sfScrollFrame1.VerticalScrollBar.Style.ThumbHoverColor = Color.DarkGray;

// Pressed state (mouse clicking/dragging thumb)
sfScrollFrame1.VerticalScrollBar.Style.ThumbPressedColor = Color.Blue;

// Disabled state (when EnableThumb = false)
sfScrollFrame1.VerticalScrollBar.Style.ThumbDisabledColor = Color.LightGray;
```

### Thumb with Border

```csharp
// Add visible border to thumb
sfScrollFrame1.VerticalScrollBar.Style.ThumbBorderColor = Color.Black;
sfScrollFrame1.HorizontalScrollBar.Style.ThumbBorderColor = Color.Black;

// Create subtle border effect
sfScrollFrame1.VerticalScrollBar.Style.ThumbColor = Color.FromArgb(180, 180, 180);
sfScrollFrame1.VerticalScrollBar.Style.ThumbBorderColor = Color.FromArgb(140, 140, 140);
```

## Arrow Button Customization

### Arrow Button States

Arrow buttons have five visual states:

```csharp
// Normal state
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonBackColor = Color.LightGray;
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonForeColor = Color.Black;

// Hover state (mouse over button)
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonHoverBackColor = Color.White;
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonHoverForeColor = Color.Black;

// Pressed state (mouse clicking button)
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonPressedBackColor = Color.Blue;
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonPressedForeColor = Color.White;

// Disabled state (when EnableMaximumArrow/EnableMinimumArrow = false)
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonDisabledBackColor = Color.Silver;
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonDisabledForeColor = Color.Gray;

// Border (all states)
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonBorderColor = Color.DarkGray;
```

### Arrow Button Styling Example

```csharp
// Create blue arrow buttons with interactive feedback
private void StyleArrowButtons()
{
    // Vertical scrollbar arrows
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonBackColor = Color.FromArgb(70, 130, 180);
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonForeColor = Color.White;
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonHoverBackColor = Color.FromArgb(100, 160, 210);
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonHoverForeColor = Color.White;
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonPressedBackColor = Color.FromArgb(40, 100, 150);
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonPressedForeColor = Color.LightGray;
    sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonBorderColor = Color.FromArgb(40, 80, 120);
    
    // Horizontal scrollbar arrows (matching style)
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonBackColor = Color.FromArgb(70, 130, 180);
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonForeColor = Color.White;
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonHoverBackColor = Color.FromArgb(100, 160, 210);
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonHoverForeColor = Color.White;
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonPressedBackColor = Color.FromArgb(40, 100, 150);
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonPressedForeColor = Color.LightGray;
    sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonBorderColor = Color.FromArgb(40, 80, 120);
}
```

## Disabling Scrollbar Elements

### Disabling Arrow Buttons

Disable the minimum (top/left) or maximum (bottom/right) arrow buttons:

```csharp
// Disable minimum arrows (top for vertical, left for horizontal)
this.sfScrollFrame1.VerticalScrollBar.EnableMinimumArrow = false;
this.sfScrollFrame1.HorizontalScrollBar.EnableMinimumArrow = false;

// Disable maximum arrows (bottom for vertical, right for horizontal)
this.sfScrollFrame1.VerticalScrollBar.EnableMaximumArrow = false;
this.sfScrollFrame1.HorizontalScrollBar.EnableMaximumArrow = false;
```

**Effect:** When disabled, arrow buttons cannot be clicked and scrolling via those buttons is prevented. The buttons still appear but use disabled styling.

#### Styling Disabled Arrow Buttons

```csharp
// Disable the maximum arrow buttons
sfScrollFrame1.VerticalScrollBar.EnableMaximumArrow = false;
sfScrollFrame1.HorizontalScrollBar.EnableMaximumArrow = false;

// Set disabled button appearance
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonDisabledBackColor = Color.Silver;
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonDisabledForeColor = Color.Gray;

sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonDisabledBackColor = Color.Silver;
sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonDisabledForeColor = Color.Gray;
```

#### Practical Example: One-Way Scrolling

```csharp
// Allow scrolling down only (disable top arrow)
sfScrollFrame1.VerticalScrollBar.EnableMinimumArrow = false;
sfScrollFrame1.VerticalScrollBar.EnableMaximumArrow = true;

// Make disabled button visually distinct
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonDisabledBackColor = Color.FromArgb(200, 200, 200);
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonDisabledForeColor = Color.FromArgb(150, 150, 150);
```

### Disabling the Thumb

Disable thumb dragging while keeping arrow button scrolling:

```csharp
// Disable thumb for both scrollbars
this.sfScrollFrame1.HorizontalScrollBar.EnableThumb = false;
this.sfScrollFrame1.VerticalScrollBar.EnableThumb = false;
```

**Effect:** The thumb is still visible but cannot be dragged. Users can only scroll via:
- Arrow button clicks
- Mouse wheel
- Track clicks (if control supports it)

#### Styling Disabled Thumb

```csharp
// Disable thumb
sfScrollFrame1.VerticalScrollBar.EnableThumb = false;
sfScrollFrame1.HorizontalScrollBar.EnableThumb = false;

// Set disabled thumb color to indicate non-interactive state
sfScrollFrame1.VerticalScrollBar.Style.ThumbDisabledColor = Color.Indigo;
sfScrollFrame1.HorizontalScrollBar.Style.ThumbDisabledColor = Color.Indigo;
```

#### Use Case: Display-Only Scrolling

```csharp
// Create read-only scrollable area (no thumb dragging)
private void SetupReadOnlyScrolling()
{
    // Attach to control
    sfScrollFrame1.Control = listView1;
    
    // Disable thumb dragging
    sfScrollFrame1.VerticalScrollBar.EnableThumb = false;
    sfScrollFrame1.HorizontalScrollBar.EnableThumb = false;
    
    // Style disabled thumb to look "locked"
    sfScrollFrame1.VerticalScrollBar.Style.ThumbDisabledColor = Color.FromArgb(180, 180, 180);
    sfScrollFrame1.HorizontalScrollBar.Style.ThumbDisabledColor = Color.FromArgb(180, 180, 180);
    
    // Keep arrow buttons enabled for basic scrolling
    sfScrollFrame1.VerticalScrollBar.EnableMaximumArrow = true;
    sfScrollFrame1.VerticalScrollBar.EnableMinimumArrow = true;
}
```

## Built-in Themes

SfScrollFrame includes six professional themes that provide consistent styling across your application.

### Available Themes

1. **Office2016Colorful** - Modern Office look with accent colors
2. **Office2016White** - Clean white theme
3. **Office2016DarkGray** - Professional dark gray theme
4. **Office2016Black** - High contrast black theme
5. **Office2019Colorful** - Updated Office 2019 styling
6. **HighContrastBlack** - Accessibility-focused high contrast

### Loading Theme Assemblies

Theme assemblies must be loaded before applying themes. Load in `Program.cs`:

```csharp
using Syncfusion.WinForms.Controls;

static class Program
{
    [STAThread]
    static void Main()
    {
        // Register Syncfusion license
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR-LICENSE-KEY");
        
        // Load theme assemblies
        SfSkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);
        SfSkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2019Theme).Assembly);
        SfSkinManager.LoadAssembly(typeof(Syncfusion.HighContrastTheme.WinForms.HighContrastTheme).Assembly);
        
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new MainForm());
    }
}
```

### Theme Assembly Requirements

| Theme | Assembly | Namespace |
|-------|----------|-----------|
| Office2016Colorful, Office2016White, Office2016DarkGray, Office2016Black | `Syncfusion.Office2016Theme.WinForms` | `Syncfusion.WinForms.Themes` |
| Office2019Colorful | `Syncfusion.Office2019Theme.WinForms` | `Syncfusion.WinForms.Themes` |
| HighContrastBlack | `Syncfusion.HighContrastTheme.WinForms` | `Syncfusion.HighContrastTheme.WinForms` |

### Applying Themes

Set the `ThemeName` property to apply a theme:

#### Office2016Colorful

```csharp
// Office2016Colorful theme
this.sfScrollFrame1.ThemeName = "Office2016Colorful";
```

#### Office2016White

```csharp
// Office2016White theme
this.sfScrollFrame1.ThemeName = "Office2016White";
```

#### Office2016DarkGray

```csharp
// Office2016DarkGray theme
this.sfScrollFrame1.ThemeName = "Office2016DarkGray";
```

#### Office2016Black

```csharp
// Office2016Black theme
this.sfScrollFrame1.ThemeName = "Office2016Black";
```

#### Office2019Colorful

```csharp
// Office2019Colorful theme
this.sfScrollFrame1.ThemeName = "Office2019Colorful";
```

#### HighContrastBlack

```csharp
// HighContrastBlack theme (for accessibility)
this.sfScrollFrame1.ThemeName = "HighContrastBlack";
```

### Complete Theme Implementation

```csharp
// In Program.cs
using System;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

namespace MyApplication
{
    static class Program
    {
        [STAThread]
        static void Main()
        {
            // Register license
            Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR-LICENSE-KEY");
            
            // Load all theme assemblies
            SfSkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);
            SfSkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2019Theme).Assembly);
            SfSkinManager.LoadAssembly(typeof(Syncfusion.HighContrastTheme.WinForms.HighContrastTheme).Assembly);
            
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
    }
}

// In MainForm.cs
public partial class MainForm : Form
{
    private SfScrollFrame sfScrollFrame1;
    private ListView listView1;
    private ComboBox themeComboBox;

    public MainForm()
    {
        InitializeComponent();
        SetupControls();
        AddThemeSelector();
    }

    private void SetupControls()
    {
        // Create ListView
        listView1 = new ListView();
        listView1.View = View.Details;
        listView1.Size = new Size(400, 300);
        listView1.Location = new Point(20, 60);
        listView1.Columns.Add("Item", 150);
        listView1.Columns.Add("Value", 150);
        
        // Add items
        for (int i = 0; i < 50; i++)
        {
            listView1.Items.Add(new ListViewItem(new[] { $"Item {i}", $"Value {i}" }));
        }
        
        // Attach SfScrollFrame
        sfScrollFrame1 = new SfScrollFrame();
        sfScrollFrame1.Control = listView1;
        sfScrollFrame1.ThemeName = "Office2019Colorful"; // Default theme
        
        this.Controls.Add(listView1);
    }

    private void AddThemeSelector()
    {
        // Add ComboBox to switch themes
        themeComboBox = new ComboBox();
        themeComboBox.Location = new Point(20, 20);
        themeComboBox.Size = new Size(200, 25);
        themeComboBox.DropDownStyle = ComboBoxStyle.DropDownList;
        
        // Add theme options
        themeComboBox.Items.AddRange(new string[]
        {
            "Office2016Colorful",
            "Office2016White",
            "Office2016DarkGray",
            "Office2016Black",
            "Office2019Colorful",
            "HighContrastBlack"
        });
        
        themeComboBox.SelectedIndex = 4; // Office2019Colorful
        
        // Handle theme change
        themeComboBox.SelectedIndexChanged += (s, e) =>
        {
            sfScrollFrame1.ThemeName = themeComboBox.SelectedItem.ToString();
        };
        
        this.Controls.Add(themeComboBox);
    }
}
```

### Theme Troubleshooting

**Theme not applying:**
- Verify theme assembly is loaded in `Program.cs`
- Check `ThemeName` spelling (case-sensitive)
- Ensure assembly reference exists in project
- Confirm license is registered

**Mixed themes in application:**
- Apply same theme to all SfScrollFrame instances
- Consider applying theme at form level for consistency
- Use a central theme management class

**Performance with themes:**
- Load theme assemblies once at startup
- Avoid changing themes frequently at runtime
- Cache theme settings when possible

## Best Practices

1. **Consistent Styling:** Apply the same style to both horizontal and vertical scrollbars
2. **State Feedback:** Always provide distinct hover and pressed states for interactivity
3. **Accessibility:** Ensure sufficient color contrast for disabled states
4. **Theme Usage:** Prefer built-in themes over manual styling for consistency
5. **Testing:** Test all scrollbar states (normal, hover, pressed, disabled)
6. **Performance:** Set styles in form constructor, not on every paint/update
7. **Documentation:** Document custom color schemes for team consistency
