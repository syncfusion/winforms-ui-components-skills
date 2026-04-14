# Behavior settings (condensed)

Overview of commonly used behavior properties for `RadioButtonAdv` with short C# examples.

## AutoHeight
- Enables automatic height based on content (use with `WrapText`).
```csharp
radio.WrapText = true;
radio.AutoHeight = true;
radio.Width = 250;
```

## DrawFocusRectangle
- Controls keyboard focus indicator (useful for accessibility).
```csharp
radio.DrawFocusRectangle = true;  // show
radio.DrawFocusRectangle = false; // hide
```

## RaiseEventOnClick
- Controls whether `Click` fires when control is clicked. `CheckedChanged` is unaffected.
```csharp
radio.RaiseEventOnClick = true;  // default
radio.RaiseEventOnClick = false; // disables Click
```

## State value properties
- Associate values for easy retrieval: `CheckedInt`, `CheckedString`, `UncheckedInt`, `UncheckedString`.
```csharp
radio.CheckedInt = 999; radio.CheckedString = "Pro";
if (radio.Checked) { var v = radio.CheckedInt; }
```

## Best practices
- Set `Width` when using `AutoHeight`.
- Use `CheckedChanged` for selection logic; avoid heavy processing in handlers.
- For accessibility, keep focus rectangles enabled unless a clear alternative exists.

## Troubleshooting
- AutoHeight: ensure `AutoHeight=true`, `WrapText=true`, and `Width` is set.
- Focus rectangle: confirm `DrawFocusRectangle=true` and that control has keyboard focus.
- Click not firing: check `RaiseEventOnClick` and handler subscription; use `CheckedChanged` if appropriate.

