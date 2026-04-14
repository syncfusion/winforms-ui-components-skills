# Appearance customization (condensed)

Short, focused guidance for styling `RadioButtonAdv`. This file contains minimal, copy-ready C# snippets and best practices.

## Overview

Key styling features: gradients (`BackgroundStyle`, `GradientStart`, `GradientEnd`), borders (`BorderStyle`, `BorderColor`, `BorderSingle`, `Border3DStyle`), alignment (`TextContentAlignment`, `CheckAlign`), and image-based rendering (`ImageCheckBox`, `CheckedImage`, `UncheckedImage`).

## Quick examples (C#)

Gradient (horizontal):
```csharp
radio.BackgroundStyle = Syncfusion.Windows.Forms.Tools.CheckBoxAdvBackStyle.HorizontalGradient;
radio.GradientStart = Color.LightSkyBlue;
radio.GradientEnd = Color.DarkBlue;
```

Solid 2D border with hover color:
```csharp
radio.BorderStyle = BorderStyle.FixedSingle;
radio.BorderColor = Color.Navy;
radio.BorderSingle = ButtonBorderStyle.Solid;
radio.HotBorderColor = Color.DarkOrange;
```

Image-based radio button:
```csharp
radio.ImageCheckBox = true;
radio.ImageCheckBoxSize = new Size(20,20);
radio.CheckedImage = Image.FromFile("checked.png");
radio.UncheckedImage = Image.FromFile("unchecked.png");
```

Text & check alignment:
```csharp
radio.TextContentAlignment = ContentAlignment.MiddleLeft; // text
radio.CheckAlign = ContentAlignment.MiddleRight;        // circle
```

## Small runnable pattern

Creates a styled option with gradient and border:
```csharp
var r = new RadioButtonAdv { Text = "Option", Size = new Size(300,36) };
r.BackgroundStyle = Syncfusion.Windows.Forms.Tools.CheckBoxAdvBackStyle.HorizontalGradient;
r.GradientStart = Color.White;
r.GradientEnd = Color.LightGray;
r.BorderStyle = BorderStyle.FixedSingle;
r.BorderColor = Color.LightSlateGray;
this.Controls.Add(r);
```

## Best practices
- Use gradients sparingly and ensure text contrast.
- Prefer `FixedSingle` for modern UIs; use `Fixed3D` only for traditional styles.
- For image checkboxes, load images once and reuse cached instances.
- Keep control sizes adequate for borders and images.

## Troubleshooting (quick)
- Gradient not visible: ensure `BackgroundStyle` ≠ `Default` and gradient colors differ.
- Border invisible: set `BorderStyle` to `FixedSingle`/`Fixed3D` and choose contrasting `BorderColor`.
- Images missing: ensure `ImageCheckBox = true`, files exist, and image objects are not null.

## References
- See other reference files for focused topics (alignment, events, behavior).

