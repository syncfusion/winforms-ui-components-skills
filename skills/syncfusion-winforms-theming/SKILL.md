---
name: syncfusion-winforms-theming
description: Comprehensive guide for implementing Syncfusion Windows Forms SkinManager and theme application. Use this when working with WinForms themes, SkinManager, or applying Office2016/Office2019 themes to controls. This skill covers theme assemblies, VisualTheme configuration, ApplicationVisualTheme setup, and custom control styling.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Windows Forms Theming (SkinManager)

The Syncfusion Windows Forms SkinManager provides support to apply consistent themes across all Syncfusion controls in an application. It enables uniform styling for entire applications, forms, or individual controls using predefined themes or custom Theme Studio-based themes.

## When to Use This Skill

Use this skill when you need to:
- Apply themes to Syncfusion WinForms controls (Office2007, Office2010, Office2013, Office2016, Office2019, Metro)
- Set up theme assemblies and load them properly
- Apply themes to entire applications, forms, or containers
- Customize theme appearance using ThemeStyle or Style properties
- Apply themes through designer or code-based approaches
- Control whether themes override user customizations

## Component Overview

**SkinManager** is a component that manages theme application for Syncfusion Windows Forms controls. It supports:
- Multiple predefined themes (Office2007, Office2010, Office2013, Office2016, Office2019, Metro, HighContrast)
- Application-wide theme management
- Form/container-level theme application
- Individual control theme application
- Advanced styling through ThemeStyle/Style properties

**Key Assembly:** `Syncfusion.Shared.Base`

**Additional Assemblies:** Some themes require separate assemblies (Office2016Theme, Office2019Theme, HighContrastTheme)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and theme assembly requirements
- Loading theme assemblies using LoadAssembly method
- Adding SkinManager component through designer
- Adding SkinManager component through code
- Basic theme application to controls and forms
- Applying themes to entire forms and containers

### Theme Management
📄 **Read:** [references/theme-management.md](references/theme-management.md)
- Complete list of available themes
- VisualTheme property usage
- SetVisualTheme method for programmatic theme setting
- Application-wide theming using ApplicationVisualTheme
- Individual control theming using ThemeName property
- Assembly requirements for specific themes
- Theme application patterns and best practices

### Advanced Customization
📄 **Read:** [references/advanced-customization.md](references/advanced-customization.md)
- ThemeStyle property for theme customization
- Style property for Sf-prefixed controls
- CanOverrideStyle property behavior
- FontHelper customization for application-wide fonts
- Custom styling examples and patterns
- Overriding user customizations with themes

## Quick Start Example

### Basic Theme Application (Code-Based)

```csharp
using Syncfusion.Windows.Forms;

namespace MyWinFormsApp
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            
            // Create SkinManager instance
            SkinManager skinManager1 = new SkinManager(this.components);
            
            // Apply theme to entire form (all Syncfusion controls)
            skinManager1.Controls = this;
            skinManager1.VisualTheme = VisualTheme.Office2016Black;
        }
    }
}
```

### Application-Wide Theme (Program.cs)

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.WinForms.Themes;

static class Program
{
    [STAThread]
    static void Main()
    {
        // Load theme assembly (for Office2019, Office2016, HighContrast)
        SkinManager.LoadAssembly(typeof(Office2019Theme).Assembly);
        
        // Set application-wide theme
        SkinManager.ApplicationVisualTheme = "Office2019Colorful";
        
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());
    }
}
```

## Common Patterns

### Pattern 1: Theme Entire Form

```csharp
// Apply theme to all Syncfusion controls in a form
SkinManager skinManager1 = new SkinManager(this.components);
skinManager1.Controls = this; // 'this' refers to the form
skinManager1.VisualTheme = VisualTheme.Office2016White;
```

### Pattern 2: Theme Individual Control

```csharp
// Option 1: Using SkinManager
skinManager1.Controls = treeViewAdv1;
skinManager1.VisualTheme = VisualTheme.Office2016Colorful;

// Option 2: Using ThemeName property
this.treeViewAdv1.ThemeName = "Office2019Colorful";
```

### Pattern 3: Loading Required Theme Assemblies

```csharp
// Office2019 themes
SkinManager.LoadAssembly(typeof(Office2019Theme).Assembly);

// Office2016 themes (for SfDataGrid, SfButton, etc.)
SkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);

// HighContrast themes
SkinManager.LoadAssembly(typeof(HighContrastTheme).Assembly);
```

### Pattern 4: Customize Theme Appearance

```csharp
// For Theme Studio themes, customize using ThemeStyle
this.treeViewAdv1.ThemeName = "Office2019Colorful";
this.treeViewAdv1.ThemeStyle.BackColor = System.Drawing.Color.White;
this.treeViewAdv1.ThemeStyle.BorderColor = System.Drawing.Color.SteelBlue;
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.TextColor = System.Drawing.Color.Red;
```

## Key Properties and Methods

### SkinManager Properties

| Property | Description | Type |
|----------|-------------|------|
| `Controls` | Specifies the parent control/form for which theme is applied | Control |
| `VisualTheme` | Specifies the theme style to apply | Enum (VisualTheme) |
| `ApplicationVisualTheme` | Sets theme for entire application (static property) | String |

### SkinManager Methods

| Method | Description | Parameters |
|--------|-------------|------------|
| `SetVisualTheme(Control, VisualTheme)` | Applies theme to specified control | Control, VisualTheme enum |
| `SetVisualTheme(Control, String)` | Applies theme to specified control | Control, theme name string |
| `LoadAssembly(Assembly)` | Loads theme assembly before applying theme | Assembly object |

### Control-Level Properties

| Property | Description | Available On |
|----------|-------------|--------------|
| `ThemeName` | Theme name to apply to individual control | All Syncfusion controls |
| `ThemeStyle` | Customize theme appearance properties | Non-Sf controls |
| `Style` | Customize theme appearance properties | Sf-prefixed controls |
| `CanOverrideStyle` | Allow theme to override user customizations | Theme Studio compatible controls |

## Available Themes

### Predefined Themes (Built-in)
- **Office2007**: Blue, Black, Silver, Managed
- **Office2010**: Blue, Black, Silver, Managed
- **Office2013**: White, Dark Gray, Black, Colorful
- **Metro**

### Separate Assembly Themes
- **Office2016**: White, Dark Gray, Black, Colorful (requires `Syncfusion.Office2016Theme.WinForms.dll`)
- **Office2019**: Colorful (requires `Syncfusion.Office2019Theme.WinForms.dll`)
- **HighContrast**: Black (requires `Syncfusion.HighContrastTheme.WinForms.dll`)

## Common Use Cases

1. **Uniform Application Styling**: Apply consistent theme across all forms and controls
2. **Modern UI Appearance**: Use Office2016/2019 themes for contemporary look
3. **Dark Mode Support**: Implement dark themes (Office2016Black, Office2016DarkGray)
4. **Accessibility Compliance**: Use HighContrast themes for accessibility
5. **Dynamic Theme Switching**: Change themes at runtime based on user preferences
6. **Per-Form Theming**: Apply different themes to different forms in the same application

## Important Notes

- **Assembly Loading**: Always call `LoadAssembly()` before applying themes that require separate assemblies
- **Application-Wide Theme**: Set `ApplicationVisualTheme` before main form initialization (in Program.cs)
- **Container Theming**: Apply theme to form/container instead of individual controls for efficiency
- **Override Behavior**: Themes respect `CanOverrideStyle` property to control customization overrides

## Troubleshooting

**Theme not applying:**
- Verify theme assembly is loaded using `LoadAssembly()`
- Check that `Controls` property is set correctly
- Ensure theme name spelling is exact (case-sensitive)

**Custom theme not working:**
- Confirm custom theme assembly is referenced in project
- Verify `LoadAssembly()` is called before theme application
- Check that theme name matches assembly name exactly

**Appearance customization not taking effect:**
- Verify using Theme Studio-based themes (Office2019Colorful, HighContrastBlack)
- Check `CanOverrideStyle` is set to `false` if you want to preserve customizations
- Use `ThemeStyle` for non-Sf controls, `Style` for Sf-prefixed controls
