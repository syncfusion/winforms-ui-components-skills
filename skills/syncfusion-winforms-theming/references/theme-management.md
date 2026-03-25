# Theme Management

## Table of Contents
- [Available Themes](#available-themes)
- [VisualTheme Property](#visualtheme-property)
- [SetVisualTheme Method](#setvisualtheme-method)
- [Application-Wide Theming](#application-wide-theming)
- [Individual Control Theming](#individual-control-theming)
- [Theme Application Patterns](#theme-application-patterns)

This guide covers all methods of applying and managing themes in Windows Forms applications using the SkinManager component.

## Available Themes

Syncfusion Windows Forms supports multiple predefined themes across different Office versions and styles.

### Office2007 Themes (Built-in)

No separate assembly required. Included in control assemblies.

```csharp
VisualTheme.Office2007Blue
VisualTheme.Office2007Black
VisualTheme.Office2007Silver
VisualTheme.Office2007Managed
```

### Office2010 Themes (Built-in)

No separate assembly required. Included in control assemblies.

```csharp
VisualTheme.Office2010Blue
VisualTheme.Office2010Black
VisualTheme.Office2010Silver
VisualTheme.Office2010Managed
```

### Office2013 Themes (Built-in)

No separate assembly required. Included in control assemblies.

```csharp
VisualTheme.Office2013White
VisualTheme.Office2013DarkGray
VisualTheme.Office2013Black
VisualTheme.Office2013LightGray
```

### Office2016 Themes (Separate Assembly)

Requires: `Syncfusion.Office2016Theme.WinForms.dll` (only for specific Sf-prefixed controls: SfDataGrid, SfButton, SfDateTimeEdit, SfNumericTextBox, SfToolTip, SfSmithChart)

```csharp
// Load assembly first
SkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);

// Available themes
VisualTheme.Office2016White
VisualTheme.Office2016DarkGray
VisualTheme.Office2016Black
VisualTheme.Office2016Colorful
```

### Office2019 Themes (Separate Assembly)

Requires: `Syncfusion.Office2019Theme.WinForms.dll`

```csharp
// Load assembly first
SkinManager.LoadAssembly(typeof(Office2019Theme).Assembly);

// Available theme
VisualTheme.Office2019Colorful
```

### Metro Theme (Built-in)

No separate assembly required.

```csharp
VisualTheme.Metro
```

### HighContrast Theme (Separate Assembly)

Requires: `Syncfusion.HighContrastTheme.WinForms.dll`

```csharp
// Load assembly first
SkinManager.LoadAssembly(typeof(HighContrastTheme).Assembly);

// Available theme
// Use via ThemeName property with string "HighContrastBlack"
```

## VisualTheme Property

The `VisualTheme` property is the primary way to apply themes using the SkinManager component.

### Basic Usage

```csharp
using Syncfusion.Windows.Forms;

SkinManager skinManager1 = new SkinManager(this.components);
skinManager1.Controls = this; // Apply to entire form
skinManager1.VisualTheme = VisualTheme.Office2016Black;
```

```vb
' VB.NET
Imports Syncfusion.Windows.Forms

Dim skinManager1 As New SkinManager(Me.components)
skinManager1.Controls = Me ' Apply to entire form
skinManager1.VisualTheme = VisualTheme.Office2016Black
```

### Applying to Specific Controls

```csharp
// Apply theme to a specific control
skinManager1.Controls = treeViewAdv1;
skinManager1.VisualTheme = VisualTheme.Office2016Colorful;
```

### Applying to Containers

```csharp
// Apply theme to all controls in a panel
skinManager1.Controls = panel1;
skinManager1.VisualTheme = VisualTheme.Office2016White;
```

### Dynamic Theme Switching

```csharp
private void ChangeTheme(VisualTheme newTheme)
{
    skinManager1.VisualTheme = newTheme;
    // Theme applies immediately to all controls in skinManager1.Controls
}

// Example: Theme selector
private void comboBoxTheme_SelectedIndexChanged(object sender, EventArgs e)
{
    switch (comboBoxTheme.SelectedIndex)
    {
        case 0:
            skinManager1.VisualTheme = VisualTheme.Office2016White;
            break;
        case 1:
            skinManager1.VisualTheme = VisualTheme.Office2016Black;
            break;
        case 2:
            skinManager1.VisualTheme = VisualTheme.Office2016Colorful;
            break;
        case 3:
            skinManager1.VisualTheme = VisualTheme.Office2019Colorful;
            break;
    }
}
```

## SetVisualTheme Method

The `SetVisualTheme` method provides an alternative way to apply themes programmatically. It has two overloads.

### Method Overloads

```csharp
// Overload 1: Using VisualTheme enum
public static void SetVisualTheme(Control control, VisualTheme theme)

// Overload 2: Using theme name string
public static void SetVisualTheme(Control control, string themeName)
```

### Using VisualTheme Enum

```csharp
using Syncfusion.Windows.Forms;

// Apply theme using enum
SkinManager.SetVisualTheme(this, VisualTheme.Office2016Black);
```

```vb
' VB.NET
Imports Syncfusion.Windows.Forms

' Apply theme using enum
SkinManager.SetVisualTheme(Me, VisualTheme.Office2016Black)
```

### Using Theme Name String

```csharp
// Apply theme using string name
SkinManager.SetVisualTheme(this, "Office2016Colorful");
```

```vb
' VB.NET
' Apply theme using string name
SkinManager.SetVisualTheme(Me, "Office2016Colorful")
```

### Comparison: Property vs Method

| Approach | Code Style | Use Case |
|----------|-----------|----------|
| `VisualTheme` property | `skinManager1.VisualTheme = theme;` | When using SkinManager instance, need to change theme later |
| `SetVisualTheme` method | `SkinManager.SetVisualTheme(control, theme);` | Static method, one-time theme application, no instance needed |

## Application-Wide Theming

Apply theme to all controls and forms in the entire application using the `ApplicationVisualTheme` property.

### Setting Application-Wide Theme

**Important:** Set `ApplicationVisualTheme` in `Program.cs` **before** the main form is initialized.

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.WinForms.Themes;

static class Program
{
    [STAThread]
    static void Main()
    {
        // Load theme assembly if needed
        SkinManager.LoadAssembly(typeof(Office2019Theme).Assembly);
        
        // Set application-wide theme (BEFORE form initialization)
        SkinManager.ApplicationVisualTheme = "Office2019Colorful";
        
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());
    }
}
```

```vb
' VB.NET
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms
Imports Syncfusion.WinForms.Themes

Module Program
    <STAThread>
    Private Sub Main()
        ' Load theme assembly if needed
        SkinManager.LoadAssembly(GetType(Office2019Theme).Assembly)
        
        ' Set application-wide theme (BEFORE form initialization)
        SkinManager.ApplicationVisualTheme = "Office2019Colorful"
        
        Application.EnableVisualStyles()
        Application.SetCompatibleTextRenderingDefault(False)
        Application.Run(New Form1())
    End Sub
End Module
```

### Benefits of Application-Wide Theming

- Single line of code themes entire application
- All forms and controls automatically themed
- No need for SkinManager instances in each form
- Consistent appearance across all windows
- Simplifies theme management

### Theme Names for ApplicationVisualTheme

Use string names (not enum values):

```csharp
// Valid theme names
"Office2007Blue"
"Office2010Black"
"Office2013White"
"Office2016Colorful"
"Office2019Colorful"
"Metro"
"HighContrastBlack"
```

## Individual Control Theming

Apply theme to specific controls without affecting others using the `ThemeName` property.

### Using ThemeName Property

```csharp
// Theme individual control
this.treeViewAdv1.ThemeName = "Office2019Colorful";
```

```vb
' VB.NET
' Theme individual control
Me.treeViewAdv1.ThemeName = "Office2019Colorful"
```

### Multiple Controls with Different Themes

```csharp
// Different themes for different controls
this.treeViewAdv1.ThemeName = "Office2016Black";
this.ribbonControlAdv1.ThemeName = "Office2016Colorful";
this.sfDataGrid1.ThemeName = "Office2019Colorful";
```

### Loading Assembly for ThemeName

When using `ThemeName` property with themes requiring separate assemblies, still call `LoadAssembly()`:

```csharp
// In Program.cs or form constructor
SkinManager.LoadAssembly(typeof(Office2019Theme).Assembly);

// Then in form
this.treeViewAdv1.ThemeName = "Office2019Colorful";
```

### When to Use Individual Control Theming

- Mixed theme requirements within a single form
- Highlighting specific controls with different themes
- Gradual theme migration (theme controls one at a time)
- Special styling for particular UI sections

## Theme Application Patterns

### Pattern 1: Single Form, Single Theme

**Best Approach:** Use SkinManager with form-level Controls assignment

```csharp
SkinManager skinManager1 = new SkinManager(this.components);
skinManager1.Controls = this;
skinManager1.VisualTheme = VisualTheme.Office2016Black;
```

### Pattern 2: Multiple Forms, Same Theme

**Best Approach:** Use ApplicationVisualTheme in Program.cs

```csharp
// Program.cs
SkinManager.ApplicationVisualTheme = "Office2016Black";
```

### Pattern 3: Multiple Forms, Different Themes

**Approach:** Each form has its own SkinManager instance

```csharp
// Form1
public partial class Form1 : Form
{
    private SkinManager skinManager1;
    
    public Form1()
    {
        InitializeComponent();
        skinManager1 = new SkinManager(this.components);
        skinManager1.Controls = this;
        skinManager1.VisualTheme = VisualTheme.Office2016White;
    }
}

// Form2
public partial class Form2 : Form
{
    private SkinManager skinManager2;
    
    public Form2()
    {
        InitializeComponent();
        skinManager2 = new SkinManager(this.components);
        skinManager2.Controls = this;
        skinManager2.VisualTheme = VisualTheme.Office2016Black;
    }
}
```

### Pattern 4: User-Selectable Theme

**Approach:** Store theme preference, apply on application start

```csharp
// Save user preference
Properties.Settings.Default.SelectedTheme = "Office2016Colorful";
Properties.Settings.Default.Save();

// Apply on startup (Program.cs)
static void Main()
{
    string userTheme = Properties.Settings.Default.SelectedTheme;
    if (!string.IsNullOrEmpty(userTheme))
    {
        SkinManager.ApplicationVisualTheme = userTheme;
    }
    
    Application.Run(new Form1());
}
```

### Pattern 5: Runtime Theme Switching

**Approach:** Change VisualTheme property dynamically

```csharp
private void btnChangeTheme_Click(object sender, EventArgs e)
{
    // Runtime theme switch
    if (skinManager1.VisualTheme == VisualTheme.Office2016White)
        skinManager1.VisualTheme = VisualTheme.Office2016Black;
    else
        skinManager1.VisualTheme = VisualTheme.Office2016White;
}
```

## Assembly Requirements Summary

| Theme Category | Assembly Required | LoadAssembly() Needed |
|----------------|-------------------|----------------------|
| Office2007 | Built-in | No |
| Office2010 | Built-in | No |
| Office2013 | Built-in | No |
| Metro | Built-in | No |
| Office2016 | Syncfusion.Office2016Theme.WinForms.dll | Yes (for Sf controls only) |
| Office2019 | Syncfusion.Office2019Theme.WinForms.dll | Yes |
| HighContrast | Syncfusion.HighContrastTheme.WinForms.dll | Yes |
| Custom (Theme Studio) | Custom exported assembly | Yes |

## Best Practices

1. **Load assemblies early**: Call `LoadAssembly()` in `Program.cs` before creating any forms
2. **Use ApplicationVisualTheme**: For uniform theming across entire application
3. **Theme at form level**: Apply to form/container rather than individual controls
4. **Cache SkinManager instances**: Create once per form, reuse for theme switching
5. **Use string names consistently**: When using ApplicationVisualTheme or ThemeName properties
6. **Test theme compatibility**: Verify all controls support the selected theme

## Troubleshooting

**Issue:** Theme not applying  
**Solution:** Ensure assembly is loaded, Controls property is set, and theme name is correct

**Issue:** Mixed theme results  
**Solution:** Check for conflicting ThemeName properties on individual controls

**Issue:** ApplicationVisualTheme not working  
**Solution:** Verify it's set before form initialization in Program.cs

**Issue:** Office2016/2019 theme error  
**Solution:** Confirm theme assembly is referenced and LoadAssembly() is called
