# Appearance and Styling

This guide covers visual customization of the MultiColumnComboBox control, including built-in themes, color schemes, and custom styling options.

## When to Read This

Read this reference when:
- Applying consistent visual themes across your application
- Matching the combo box to your brand colors
- Implementing Office-style visual themes
- Customizing colors for specific UI requirements
- Creating dark mode or high-contrast interfaces

## Available Visual Styles

The MultiColumnComboBox provides 9 built-in visual styles through the `Style` property.

### Style Options

| Style | Description | Best For |
|-------|-------------|----------|
| `Office2003` | Classic Office 2003 look | Legacy applications, Windows XP era |
| `OfficeXP` | Office XP theme | Older enterprise applications |
| `VS2005` | Visual Studio 2005 style | Development tools, technical apps |
| `Office2007` | Office 2007 with Aero | Windows Vista/7 applications |
| `Metro` | Modern flat design | Modern, minimalist interfaces |
| `Office2016Colorful` | Office 2016 blue theme | Modern Office-style apps |
| `Office2016White` | Office 2016 light theme | Clean, bright interfaces |
| `Office2016Black` | Office 2016 dark theme | Dark mode applications |
| `Office2016DarkGray` | Office 2016 gray theme | Professional, subdued interfaces |

## Basic Style Application

### Setting a Style

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
# Appearance and Styling

Short, focused guide for theming `MultiColumnComboBox`. Examples are C#-first; a single VB parity note is included.

## Core Points
- Property: `Style` — choose built-in `VisualStyle` (Metro, Office2016Colorful/White/Black/DarkGray, Office2007, Office2003, OfficeXP, VS2005).
- Helper: `Office2007ColorTheme` when `Style = VisualStyle.Office2007`.
- Use `Office2007Colors.ApplyManagedColors(this, Color)` for brand colors.

## Minimal examples

**C# — set a theme and brand color**
```csharp
multiColumnComboBox1.Style = VisualStyle.Office2016Colorful;
// Brand color (Managed theme)
multiColumnComboBox1.Style = VisualStyle.Office2007;
multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.FromArgb(0,120,215));
```

**VB.NET parity (single-line note)**
```vbnet
' VB parity: set `Style` and optionally call `Office2007Colors.ApplyManagedColors(Me, Color.Blue)`
```

## Best practices
- Apply theme once (Form constructor or Load event).  
- For dark mode use `Office2016Black` / `Office2016DarkGray`.  
- Call `Refresh()` after changing style when needed.

---

See `getting-started.md` and `data-binding.md` for usage and examples.
' Office 2016 White (clean light theme)
