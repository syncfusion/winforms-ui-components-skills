# Visual Styles and Theming

This guide covers the built-in visual styles and theming options for the SplitButton control, including Office 2016/2019 themes and style customization.

## Overview

The SplitButton control provides six built-in visual styles that match modern Microsoft Office design patterns. You can apply these styles using either the `Style` property or the `ThemeName` property.

## Built-in Visual Styles

The following visual styles are available through the `Style` property:

| Style | Description | Best For |
|-------|-------------|----------|
| **Office2016White** | Clean white background with subtle borders | Light-themed applications |
| **Office2016Black** | Dark background with light text | Dark-themed applications |
| **Office2016DarkGray** | Medium gray background | Professional applications |
| **Office2016Colorful** | Accent colors with white background | Modern, vibrant applications |
| **Metro** | Flat design with minimal borders | Windows 8/10 style apps |
| **Default** | Standard Windows Forms appearance | Legacy or system-native apps |

## Applying Visual Styles

### Using Style Property

The `Style` property accepts `SplitButtonVisualStyle` enum values:

**C# Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;

// Apply Office2016Colorful style
this.splitButton1.Style = SplitButtonVisualStyle.Office2016Colorful;
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms.Tools

' Apply Office2016Colorful style
Me.splitButton1.Style = SplitButtonVisualStyle.Office2016Colorful
```

### Using ThemeName Property

The `ThemeName` property accepts theme names as strings:

**C# Example:**
```csharp
// Apply Office2019Colorful theme
this.splitButton1.ThemeName = "Office2019Colorful";
```

**VB.NET Example:**
```vb
' Apply Office2019Colorful theme
Me.splitButton1.ThemeName = "Office2019Colorful"
```

## Style Examples

### Office2016White Style

Clean, light appearance suitable for bright interfaces:

```csharp
SplitButton btn = new SplitButton
{
    Text = "Save",
    Size = new Size(100, 35),
    Location = new Point(20, 20),
    Style = SplitButtonVisualStyle.Office2016White
};

btn.DropDownItems.Add(new ToolStripMenuItem("Save"));
btn.DropDownItems.Add(new ToolStripMenuItem("Save As..."));

this.Controls.Add(btn);
```

**Appearance:** White background, subtle gray borders, clean typography

### Office2016Black Style

Dark theme for low-light environments:

```csharp
SplitButton btn = new SplitButton
{
    Text = "Options",
    Size = new Size(100, 35),
    Location = new Point(20, 20),
    Style = SplitButtonVisualStyle.Office2016Black
};

btn.DropDownItems.Add(new ToolStripMenuItem("Option 1"));
btn.DropDownItems.Add(new ToolStripMenuItem("Option 2"));

this.Controls.Add(btn);
```

**Appearance:** Dark background, light text, high contrast

### Office2016DarkGray Style

Professional medium-gray theme:

```csharp
SplitButton btn = new SplitButton
{
    Text = "Tools",
    Size = new Size(100, 35),
    Location = new Point(20, 20),
    Style = SplitButtonVisualStyle.Office2016DarkGray
};

this.Controls.Add(btn);
```

**Appearance:** Medium gray background, balanced contrast

### Office2016Colorful Style

Vibrant, modern appearance with accent colors:

```csharp
SplitButton btn = new SplitButton
{
    Text = "Actions",
    Size = new Size(100, 35),
    Location = new Point(20, 20),
    Style = SplitButtonVisualStyle.Office2016Colorful
};

this.Controls.Add(btn);
```

**Appearance:** Colorful accents, white background, modern look

### Metro Style

Flat, minimalist Windows 8/10 design:

```csharp
SplitButton btn = new SplitButton
{
    Text = "Send",
    Size = new Size(100, 35),
    Location = new Point(20, 20),
    Style = SplitButtonVisualStyle.Metro
};

this.Controls.Add(btn);
```

**Appearance:** Flat design, minimal borders, clean edges

### Default Style

Standard Windows Forms appearance:

```csharp
SplitButton btn = new SplitButton
{
    Text = "Default",
    Size = new Size(100, 35),
    Location = new Point(20, 20),
    Style = SplitButtonVisualStyle.Default
};

this.Controls.Add(btn);
```

**Appearance:** System-native Windows Forms button style

## Theme Application Patterns

### Consistent Application-Wide Theming

Apply the same theme to all SplitButtons in your application:

```csharp
public class ThemeManager
{
    public static SplitButtonVisualStyle CurrentTheme = SplitButtonVisualStyle.Office2016Colorful;
    
    public static void ApplyTheme(SplitButton button)
    {
        button.Style = CurrentTheme;
    }
    
    public static void SetApplicationTheme(SplitButtonVisualStyle theme)
    {
        CurrentTheme = theme;
        // Apply to all existing buttons
        ApplyThemeToAllButtons(Application.OpenForms);
    }
    
    private static void ApplyThemeToAllButtons(FormCollection forms)
    {
        foreach (Form form in forms)
        {
            foreach (Control control in form.Controls)
            {
                if (control is SplitButton splitButton)
                {
                    splitButton.Style = CurrentTheme;
                }
            }
        }
    }
}

// Usage
ThemeManager.SetApplicationTheme(SplitButtonVisualStyle.Office2016Black);
```

### Dynamic Theme Switching

Allow users to change theme at runtime:

```csharp
public Form1()
{
    InitializeComponent();
    
    // Create theme selector
    SplitButton themeSelector = new SplitButton
    {
        Text = "Theme",
        Size = new Size(120, 35),
        Location = new Point(10, 10)
    };
    
    themeSelector.DropDownItems.Add(new ToolStripMenuItem("Office2016White"));
    themeSelector.DropDownItems.Add(new ToolStripMenuItem("Office2016Black"));
    themeSelector.DropDownItems.Add(new ToolStripMenuItem("Office2016DarkGray"));
    themeSelector.DropDownItems.Add(new ToolStripMenuItem("Office2016Colorful"));
    themeSelector.DropDownItems.Add(new ToolStripMenuItem("Metro"));
    
    themeSelector.DropDownItemClicked += (s, e) => {
        SplitButtonVisualStyle selectedStyle = (SplitButtonVisualStyle)
            Enum.Parse(typeof(SplitButtonVisualStyle), e.ClickedItem.Text);
        
        ApplyThemeToForm(selectedStyle);
    };
    
    this.Controls.Add(themeSelector);
}

private void ApplyThemeToForm(SplitButtonVisualStyle style)
{
    foreach (Control control in this.Controls)
    {
        if (control is SplitButton splitButton)
        {
            splitButton.Style = style;
        }
    }
}
```

### Theme Based on System Settings

Match system dark/light mode:

```csharp
using Microsoft.Win32;

private void SetThemeFromSystem()
{
    // Check Windows theme
    bool isDarkMode = IsDarkModeEnabled();
    
    foreach (Control control in this.Controls)
    {
        if (control is SplitButton splitButton)
        {
            splitButton.Style = isDarkMode 
                ? SplitButtonVisualStyle.Office2016Black 
                : SplitButtonVisualStyle.Office2016White;
        }
    }
}

private bool IsDarkModeEnabled()
{
    try
    {
        using (RegistryKey key = Registry.CurrentUser.OpenSubKey(
            @"Software\Microsoft\Windows\CurrentVersion\Themes\Personalize"))
        {
            object value = key?.GetValue("AppsUseLightTheme");
            return value is int i && i == 0;
        }
    }
    catch
    {
        return false; // Default to light mode if unable to detect
    }
}
```

## Combining Styles with Other Features

### Styled Toggle Button

Apply theme to toggle mode button:

```csharp
SplitButton toggleBtn = new SplitButton
{
    Text = "Filter",
    ButtonMode = ButtonMode.Toggle,
    IsButtonChecked = false,
    Style = SplitButtonVisualStyle.Office2016Colorful,
    Size = new Size(100, 35)
};

toggleBtn.DropDownItems.Add(new ToolStripMenuItem("All"));
toggleBtn.DropDownItems.Add(new ToolStripMenuItem("Active"));
toggleBtn.DropDownItems.Add(new ToolStripMenuItem("Archived"));

this.Controls.Add(toggleBtn);
```

### Styled Dynamic Caption Button

Combine theming with dynamic caption updates:

```csharp
SplitButton captionBtn = new SplitButton
{
    Text = "Select Option",
    Style = SplitButtonVisualStyle.Office2016DarkGray,
    Size = new Size(140, 35)
};

captionBtn.DropDownItems.Add(new ToolStripMenuItem("Option A"));
captionBtn.DropDownItems.Add(new ToolStripMenuItem("Option B"));

captionBtn.DropDownItemClicked += (s, e) => {
    captionBtn.Text = e.ClickedItem.Text;
};

this.Controls.Add(captionBtn);
```

## Theme Configuration at Design Time

### Using Designer

1. Select the SplitButton control in the designer
2. Locate the **Style** property in the Properties window
3. Click the dropdown and select desired style
4. The control updates immediately in the designer

### Using ThemeName in Designer

1. Select the SplitButton control
2. Locate the **ThemeName** property
3. Type the theme name: "Office2019Colorful", "Office2016White", etc.
4. Press Enter to apply

## Best Practices

**Theme Selection:**
- **Office2016White:** Best for light-themed, productivity applications
- **Office2016Black:** Use for dark mode or low-light environments
- **Office2016DarkGray:** Professional, neutral appearance
- **Office2016Colorful:** Modern, vibrant applications with color accents
- **Metro:** Flat design, Windows 8/10 style applications
- **Default:** Legacy applications or when system-native look is required

**Consistency:**
- Apply the same theme to all Syncfusion controls in your application
- Don't mix themes within the same form or feature area
- Match button themes to your application's overall design language

**Performance:**
- Set Style property once during initialization
- Avoid repeatedly changing styles during runtime
- Use theme constants or enums rather than string-based ThemeName when possible

**Accessibility:**
- Prefer high-contrast themes (White or Black) for better readability
- Test themes with users who have visual impairments
- Ensure text remains readable on all theme backgrounds

## Troubleshooting

**Issue: Style not applying**
- Verify correct namespace is imported (`Syncfusion.Windows.Forms.Tools`)
- Ensure Style property is set after control initialization
- Check that ThemeName string matches exactly (case-sensitive)

**Issue: Theme looks incorrect**
- Confirm all Syncfusion assemblies are the same version
- Check if custom rendering is overriding theme (Renderer property)
- Verify control is fully initialized before applying theme

**Issue: Inconsistent themes across controls**
- Apply theme to all controls using a centralized theme manager
- Check for hardcoded Style assignments in different code locations
- Ensure Designer-generated code isn't overriding runtime theme settings

**Issue: ThemeName vs Style conflict**
- Don't set both ThemeName and Style properties simultaneously
- ThemeName takes precedence when both are set
- Use one approach consistently throughout your application

## Next Steps

- **Advanced Customization:** Read [advanced-customization.md](advanced-customization.md) for complete custom rendering beyond built-in themes
- **Button Modes:** Read [button-modes.md](button-modes.md) to understand how themes interact with toggle states
- **Getting Started:** Return to [getting-started.md](getting-started.md) for basic setup
