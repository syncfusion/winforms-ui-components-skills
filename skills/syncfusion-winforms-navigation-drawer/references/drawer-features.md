```markdown
# Drawer Features and Configuration (Condensed)

This page summarizes the NavigationDrawer core features: content area, drawer view, transitions, positions, sizing, and recommended values.

## Table of Contents
- [ContentView](#contentview)
- [DrawerView](#drawerview)
- [Transition Types](#transition-types)
  - [SlideOnTop](#slideontop)
  - [Push](#push)
  - [Reveal](#reveal)
- [Position Configuration](#position-configuration)
- [Drawer Dimensions](#drawer-dimensions)
- [Toggle Drawer](#toggle-drawer)
- [Animation Duration](#animation-duration)
- [Best Practices](#best-practices)

## ContentView

Add your main app UI to `navigationDrawer1.ContentViewContainer`. Use `Dock = DockStyle.Fill` for full-size content.

```csharp
var richTextBox = new RichTextBox { Dock = DockStyle.Fill, Text = "Main content" };
navigationDrawer1.ContentViewContainer.Controls.Add(richTextBox);
```

## DrawerView

Use `DrawerHeader` and `DrawerMenuItem` inside `navigationDrawer1.Items` for header and menu items. Manage items via the `Items` collection.

## Transition Types

- SlideOnTop: overlays content (`Transition.SlideOnTop`).
- Push: pushes content aside (`Transition.Push`).
- Reveal: content slides to reveal drawer (`Transition.Reveal`).

```csharp
navigationDrawer1.Transition = Transition.SlideOnTop; // SlideOnTop, Push, or Reveal
```

## Position Configuration

Set `Position` to `SlidePosition.Left|Right|Top|Bottom` and adjust `DrawerWidth`/`DrawerHeight` accordingly.

```csharp
navigationDrawer1.Position = SlidePosition.Left;
```

## Drawer Dimensions

Recommended widths/heights: Left/Right drawers typically 250–300px (min 200px), Top/Bottom drawers 150–250px height. Use relative sizing for responsiveness.

## Toggle Drawer

Use `ToggleDrawer()` or wire a button click or keyboard shortcut to call it.

```csharp
navigationDrawer1.ToggleDrawer();
```

## Animation Duration

Set `AnimationDuration` in milliseconds. Typical values: 100–300ms.

```csharp
navigationDrawer1.AnimationDuration = 200;
```

## Best Practices

- Avoid heavy work during animations — defer work to the `Opened` event.
- For very large menus prefer pagination or lazy-loading rather than rendering all items.
- Keep content responsive by adjusting sizes on form resize.

``` 
