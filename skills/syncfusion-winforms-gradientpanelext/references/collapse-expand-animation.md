# Collapse and Expand Animation

Guide to implementing animated collapse/expand functionality in GradientPanelExt for collapsible panel sections.

## Overview

GradientPanelExt supports animated collapse and expand operations when used with a **CollapsePrimitive**. This feature allows panels to smoothly transition between full and collapsed states.

**Requirements:**
- **CollapsePrimitive** must be added to panel's Primitives collection
- Animation properties configure the transition behavior

---

## Animation Properties

### Animated Property

Enables or disables collapse/expand animation.

**Property Type:** `bool`  
**Default Value:** `false` (instant collapse, no animation)

**C# Example:**

# Collapse and Expand Animation (trimmed)

Compact guide to animation settings for `GradientPanelExt`. Keeps one VB sample.

## Key Properties (C#)

```csharp
// Enable animation
gradientPanel.Animated = true;
gradientPanel.AnimationDelay = 0;   // ms
gradientPanel.AnimationSpeed = 3;   // higher = faster
```

**VB (compact):**
```vb
' Single compact VB example
gradientPanel.Animated = True
gradientPanel.AnimationDelay = 0
gradientPanel.AnimationSpeed = 3
```

## Usage (C# compact)

```csharp
var panel = new GradientPanelExt { Size = new Size(400,250), Animated = true, AnimationSpeed = 3 };
var collapse = new CollapsePrimitive { Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top, Position = 350, Size = new Size(30,30) };
collapse.CollapseImage = Properties.Resources.CollapseIcon;
collapse.ExpandImage = Properties.Resources.ExpandIcon;
panel.Primitives.Add(collapse);
this.Controls.Add(panel);
```

## Notes
- Use `AnimationSpeed` to tune feel; `AnimationDelay` can prevent accidental toggles.
- For performance-critical UIs, consider `Animated = false`.

## Related
- Primitives: [primitives.md](primitives.md)

**Solution:** Reduce or remove AnimationDelay

```csharp
panel.AnimationDelay = 0;  // Immediate response
```

---

## Performance Considerations

- **Animation = true**: Uses slightly more CPU during collapse/expand
- **Animation = false**: Instant, no CPU overhead
- **AnimationSpeed**: Higher values complete faster, less total CPU time
- **For many panels**: Consider disabling animation if performance critical

---

## Related Topics

- **Primitives**: CollapsePrimitive setup → [primitives.md](primitives.md)
- **Getting Started**: Basic panel setup → [getting-started.md](getting-started.md)
- **Events**: Handling collapse events → [scroll-settings-events.md](scroll-settings-events.md)
