# Events (condensed)

`RadioButtonAdv` exposes two primary events useful for selection logic: `CheckChanged` (fires when a radio's `Checked` changes) and `GroupCheckChanged` (fires for all radios in a container when any changes).

## Basic handlers (C#)

CheckChanged - subscribe and handle the checked state:
```csharp
radio.CheckChanged += (s,e) => {
    var r = s as RadioButtonAdv;
    if (r != null && r.Checked) MessageBox.Show($"Selected: {r.Text}");
};
```

GroupCheckChanged - monitor group changes:
```csharp
radio.GroupCheckChanged += (s,e) => {
    var r = s as RadioButtonAdv;
    Console.WriteLine($"{r.Text}: Checked={r.Checked}");
};
```

## Patterns
- Single handler for many radios: attach the same `CheckChanged` handler to multiple controls and use `sender` or `Tag` to differentiate.
- Use `Tag` to associate data (e.g., price or ID) with radio buttons and act on it when selected.

## Best practices
- Always verify `r.Checked` before taking action.
- Avoid heavy synchronous work in handlers; offload to background tasks.
- Unsubscribe handlers when removing dynamic controls to prevent leaks.

## Troubleshooting
- Event not firing: confirm subscription and that `Checked` actually changes.
- Duplicate firings: check for multiple subscriptions or re-entrant `Checked` assignments.

