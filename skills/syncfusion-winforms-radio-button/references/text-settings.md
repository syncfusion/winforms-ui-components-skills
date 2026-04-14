# Text settings (condensed)

Key text features for `RadioButtonAdv`: text shadowing and wrapping, with compact C# snippets.

## Properties
- `TextShadow` (bool) — enable shadow
- `ShadowColor` (Color) — shadow color
- `ShadowOffset` (Point) — shadow offset (x,y)
- `WrapText` (bool) — enable wrapping

## Quick examples (C#)

Enable shadow:
```csharp
radio.TextShadow = true;
radio.ShadowColor = Color.Gray;
radio.ShadowOffset = new Point(2,2);
```

Wrap long text with optional auto-height:
```csharp
radio.WrapText = true;
radio.AutoHeight = true; // optional
radio.Width = 200;
```

## Best practices
- Use small shadow offsets (1–3 px) for subtlety.
- Ensure shadow color contrasts with text and background.
- Combine `WrapText` with `AutoHeight` or set adequate control height.
- Prefer left-aligned text when wrapping (`TextContentAlignment = MiddleLeft`).

## Troubleshooting
- Shadow not visible: confirm `TextShadow=true`, `ShadowOffset != (0,0)`, and color contrast.
- Text truncation: enable `WrapText`, increase height, or use `AutoHeight`.

