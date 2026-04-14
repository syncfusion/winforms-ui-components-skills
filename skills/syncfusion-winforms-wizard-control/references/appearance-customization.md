# Appearance Customization

This guide covers styling the WizardControl: foreground, background, borders, built-in themes, and complete theming examples.

## When to Read This

Read this reference when:
- Customizing wizard appearance to match your application theme
- Setting fonts and colors for wizard pages
- Applying background colors or images
- Configuring border styles
- Using Syncfusion themes (Office2016, Metro, etc.)

## Foreground Settings

```csharp
// Wizard control font and color
wizardControl1.Font = new Font("Segoe UI", 9F, FontStyle.Regular);
wizardControl1.ForeColor = Color.FromArgb(60, 60, 60);

// Per-page font and color
welcomePage.Font = new Font("Segoe UI", 10F, FontStyle.Regular);
welcomePage.ForeColor = Color.Black;

// Title label
wizardControl1.Title.Font = new Font("Segoe UI", 14F, FontStyle.Bold);
wizardControl1.Title.ForeColor = Color.FromArgb(0, 51, 102);

// Description label
wizardControl1.Description.Font = new Font("Segoe UI", 9F, FontStyle.Regular);
wizardControl1.Description.ForeColor = Color.FromArgb(80, 80, 80);
```

## Background Settings

```csharp
// Solid color
wizardControl1.BackColor = Color.White;

// Background image
wizardControl1.BackgroundImage = Image.FromFile("background.png");
wizardControl1.BackgroundImageLayout = ImageLayout.Stretch;

// BannerPanel solid color
wizardControl1.BannerPanel.BackColor = Color.FromArgb(240, 245, 250);

// BannerPanel gradient
using Syncfusion.Drawing;
wizardControl1.BannerPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal, Color.AliceBlue, Color.LightSteelBlue);

// Page background
welcomePage.BackColor = Color.White;
welcomePage.BackgroundColor = new BrushInfo(GradientStyle.Vertical, Color.White, Color.FromArgb(245, 250, 255));
```

**Available GradientStyle values:** `Horizontal`, `Vertical`, `ForwardDiagonal`, `BackwardDiagonal`, `PathEllipse`, `PathRectangle`.

## Border Styles

```csharp
// WizardControl border
wizardControl1.BorderStyle = BorderStyle.FixedSingle;  // None | FixedSingle | Fixed3D

// BannerPanel 3D border
wizardControl1.BannerPanel.Border3DStyle = Border3DStyle.Flat;

// BannerPanel 2D border
wizardControl1.BannerPanel.BorderColor = Color.Gray;
wizardControl1.BannerPanel.BorderSides =
    Border3DSide.Left | Border3DSide.Top | Border3DSide.Right | Border3DSide.Bottom;
wizardControl1.BannerPanel.BorderSingle = BorderSingle.Solid;

// Page border
welcomePage.BorderStyle = BorderStyle.None;
```

## Style Property (Built-in Themes)

```csharp
// Available styles
wizardControl1.Style = WizardStyle.Office2016Colorful;
// WizardStyle.Default | Office2016Colorful | Office2016White | Office2016Black | Office2016DarkGray | Metro
```

Applying a `Style` sets consistent colors, button styles, banner appearance, and default fonts automatically.

## Complete Styling Example: Modern Light Theme

```csharp
private void ApplyModernLightTheme()
{
    wizardControl1.BackColor = Color.White;
    wizardControl1.ForeColor = Color.FromArgb(60, 60, 60);
    wizardControl1.Font = new Font("Segoe UI", 9F);
    wizardControl1.BorderStyle = BorderStyle.FixedSingle;

    wizardControl1.BannerPanel.Height = 80;
    wizardControl1.BannerPanel.BackgroundColor = new BrushInfo(
        GradientStyle.Vertical,
        Color.FromArgb(250, 250, 250),
        Color.FromArgb(235, 240, 245));
    wizardControl1.BannerPanel.BorderSingle = BorderSingle.None;

    wizardControl1.Title.Font = new Font("Segoe UI Light", 16F);
    wizardControl1.Title.ForeColor = Color.FromArgb(50, 50, 50);

    wizardControl1.Description.Font = new Font("Segoe UI", 9F);
    wizardControl1.Description.ForeColor = Color.FromArgb(100, 100, 100);

    StyleModernButton(wizardControl1.BackButton, false);
    StyleModernButton(wizardControl1.NextButton, true);
    StyleModernButton(wizardControl1.CancelButton, false);
    StyleModernButton(wizardControl1.FinishButton, true);

    foreach (WizardControlPage page in wizardControl1.WizardPages)
    {
        page.BackColor = Color.White;
        page.ForeColor = Color.FromArgb(60, 60, 60);
        page.Font = new Font("Segoe UI", 9F);
    }
}

private void StyleModernButton(Button button, bool isPrimary)
{
    button.FlatStyle = FlatStyle.Flat;
    button.Font = new Font("Segoe UI", 9F);
    button.Cursor = Cursors.Hand;

    if (isPrimary)
    {
        button.BackColor = Color.FromArgb(0, 120, 215);
        button.ForeColor = Color.White;
        button.FlatAppearance.BorderSize = 0;
        button.FlatAppearance.MouseOverBackColor = Color.FromArgb(0, 99, 177);
        button.FlatAppearance.MouseDownBackColor = Color.FromArgb(0, 78, 138);
    }
    else
    {
        button.BackColor = Color.FromArgb(245, 245, 245);
        button.ForeColor = Color.FromArgb(60, 60, 60);
        button.FlatAppearance.BorderSize = 1;
        button.FlatAppearance.BorderColor = Color.FromArgb(200, 200, 200);
        button.FlatAppearance.MouseOverBackColor = Color.FromArgb(235, 235, 235);
        button.FlatAppearance.MouseDownBackColor = Color.FromArgb(225, 225, 225);
    }
}
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Style changes not appearing | Apply styles after `Controls.Add(wizardControl1)`; call `wizardControl1.Refresh()` |
| Banner gradient not showing | Use `BackgroundColor` (not `BackColor`) with `BrushInfo`; add `using Syncfusion.Drawing` |
| Button styles reset after navigation | Re-apply styles in `BeforePageSelect` using `this.BeginInvoke(new Action(() => ApplyButtonStyles()))` |
| Font changes don''t apply to all controls | Set font on both `wizardControl1.Font` and each page's `Font` |
| BackgroundImage not visible | Set `BackgroundImageLayout = ImageLayout.Stretch`; verify file path exists |

## Next Steps

- [design-time-features.md](design-time-features.md) — designer workflow