# Getting Started with Navigation Drawer

This concise guide shows the minimal steps to add and use the NavigationDrawer control in a Windows Forms app. VB.NET examples removed for brevity; C# snippets remain.

## Install

Install the Syncfusion package that contains the control:

```powershell
Install-Package Syncfusion.Tools.Windows
```

## Add Control (C#)

```csharp
using Syncfusion.Windows.Forms.Tools;

var navigationDrawer1 = new NavigationDrawer();
navigationDrawer1.DrawerWidth = this.Width / 4;
navigationDrawer1.DrawerHeight = this.Height;
navigationDrawer1.Position = SlidePosition.Left;
navigationDrawer1.Transition = Transition.SlideOnTop;
this.Controls.Add(navigationDrawer1);
```

## Add Header and Menu Items

```csharp
navigationDrawer1.Items.Add(new DrawerHeader { Text = "Menu" });
navigationDrawer1.Items.Add(new DrawerMenuItem { Text = "Home" });
navigationDrawer1.Items.Add(new DrawerMenuItem { Text = "Settings" });
```

## Images & Resources

- Prefer embedded resources (`Properties.Resources`) for deployment.
- Avoid relative file paths in production builds.

## Quick Tips

- Use `DrawerWidth` for Left/Right drawers and `DrawerHeight` for Top/Bottom drawers.
- Use `ToggleDrawer()` to open/close programmatically.

## See also
- `drawer-features.md` — transitions and layout
- `customization.md` — themes and colors
- `events.md` — lifecycle and event handling
