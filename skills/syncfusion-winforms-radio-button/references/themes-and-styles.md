# Themes and visual styles (condensed)

Overview of key theming options for `RadioButtonAdv` with short C# examples.

## Enable Windows themes
```csharp
radio.ThemesEnabled = true;
```

## Styles
- `Default`, `Office2007`, `Metro`, `Office2016Colorful`, `Office2016White`, `Office2016Black`, `Office2016DarkGray`.

Apply a style (C#):
```csharp
radio.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2016Colorful;
```

## Office2007 color schemes
- `Office2007ColorScheme` supports `Blue`, `Silver`, `Black`, and `Managed`.
```csharp
radio.Style = RadioButtonAdvStyle.Office2007;
radio.Office2007ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Blue;
```

Apply managed colors (affects form):
```csharp
Office2007Colors.ApplyManagedColors(this, Color.Crimson);
```

## Choosing a theme (guidance)
- Modern touch apps: `Metro`
- Business/enterprise: `Office2016Colorful` or `Office2016White`
- Dark mode: `Office2016Black` / `Office2016DarkGray`
- Classic Office look: `Office2007` (Blue)

## Best practices
- Keep a single theme across the app for consistency.
- For dark themes, adjust `ForeColor` (e.g., set to `Color.White`).
- Test with Windows high-contrast themes for accessibility.

