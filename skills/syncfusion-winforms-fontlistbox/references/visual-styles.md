# Visual Styles

## Overview

The FontListBox appearance is controlled by the `VisualStyle` property, which accepts a `FontListBoxStyle` enum value. The full enum path is `Syncfusion.Windows.Forms.Tools.FontListBox.FontListBoxStyle`.

---

## Available Styles

| `FontListBoxStyle` Value | Appearance |
|---|---|
| `Default` | Standard system appearance |
| `Metro` | Flat Modern/Metro UI style |
| `Office2016Colorful` | Office 2016 colorful theme |
| `Office2016White` | Office 2016 white theme |
| `Office2016Black` | Office 2016 black theme |
| `Office2016DarkGray` | Office 2016 dark gray theme |

---

## Setting the Visual Style

**C#:**
```csharp
this.fontListBox1.VisualStyle =
    Syncfusion.Windows.Forms.Tools.FontListBox.FontListBoxStyle.Office2016Black;
```

**VB.NET:**
```vb
Me.fontListBox1.VisualStyle =
    Syncfusion.Windows.Forms.Tools.FontListBox.FontListBoxStyle.Office2016Black
```

> Note the nested enum path: `FontListBox.FontListBoxStyle` (the enum is nested inside the `FontListBox` class, not at the `Tools` namespace level directly).

---

## Examples per Style

**Office 2016 Colorful:**
```csharp
this.fontListBox1.VisualStyle =
    Syncfusion.Windows.Forms.Tools.FontListBox.FontListBoxStyle.Office2016Colorful;
```

**Metro:**
```csharp
this.fontListBox1.VisualStyle =
    Syncfusion.Windows.Forms.Tools.FontListBox.FontListBoxStyle.Metro;
```

**Default (system appearance):**
```csharp
this.fontListBox1.VisualStyle =
    Syncfusion.Windows.Forms.Tools.FontListBox.FontListBoxStyle.Default;
```
