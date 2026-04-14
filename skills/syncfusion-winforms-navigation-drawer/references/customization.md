# Customization and Styling

This guide covers visual customization options for the Navigation Drawer, including built-in themes and custom color configurations.

## Built-in Themes

The Navigation Drawer control includes five built-in Office 2016-inspired themes for professional appearance. Set the theme using the `Style` property.

### Default Theme

The default theme provides a clean, neutral appearance.

```csharp
this.navigationDrawer1.Style = NavigationDrawerStyle.Default;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Style = NavigationDrawerStyle.Default
```

![Default theme](../assets/Style_img1.png)

**Characteristics:**
- Neutral gray color scheme
- Standard borders and spacing
- Suitable for any application type
- Good contrast for readability

### Office2016Colorful Theme

A vibrant theme with blue accents inspired by Office 2016.

```csharp
this.navigationDrawer1.Style = NavigationDrawerStyle.Office2016Colorful;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Style = NavigationDrawerStyle.Office2016Colorful
```markdown
# Customization and Styling (Condensed)

This page covers theming and basic item-level styling for the NavigationDrawer. VB.NET blocks and large examples removed to save tokens.

## Built-in Themes

Set a built-in theme via the `Style` property. Typical options: `Default`, `Office2016Colorful`, `Office2016White`, `Office2016DarkGray`, `Office2016Black`.

```csharp
navigationDrawer1.Style = NavigationDrawerStyle.Office2016Colorful;
```

## Simple Color Customization

Customize individual `DrawerMenuItem` colors (minimal example):

```csharp
var item = new DrawerMenuItem { Text = "Home", DefaultColor = Color.LightBlue };
item.HoverColor = Color.SkyBlue;
navigationDrawer1.Items.Add(item);
```

Use `ForeColor` adjustments or a simple luminance check to ensure readable contrast when changing backgrounds.

## Theme Switching (minimal)

Save a small preference (string) and apply on startup. Prefer `System.Text.Json` or a settings value for persistence — declare dependency if using external packages.

## Best Practices

- Prefer embedded resources for images and icons.
- Avoid frequent color recalculations during animations.
- Keep custom styling minimal and consistent with chosen theme.

``` 
this.navigationDrawer1.Style = NavigationDrawerStyle.Office2016DarkGray;
