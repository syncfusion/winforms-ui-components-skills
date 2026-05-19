# Getting Started with RadialMenu

This guide walks you through setting up the Syncfusion WinForms RadialMenu control in your Windows Forms application, from installation to creating your first functional radial menu.

## Assembly Dependencies

Before using the RadialMenu control, you need to add references to the following Syncfusion assemblies:

**Required DLL References:**
- **Syncfusion.Grid.Base.dll** - Base grid functionality
- **Syncfusion.Grid.Windows.dll** - Windows Forms grid components
- **Syncfusion.Shared.Base.dll** - Shared base utilities
- **Syncfusion.Shared.Windows.dll** - Windows Forms shared components
- **Syncfusion.Tools.Base.dll** - Tools base functionality
- **Syncfusion.Tools.Windows.dll** - Windows Forms tools (contains RadialMenu)

These assemblies can be referenced either by adding the DLLs directly or by installing the NuGet package.

## NuGet Package

```text
Syncfusion.Tools.Windows
```

```markdown
# Getting Started — RadialMenu (condensed)

Quick: install the Syncfusion WinForms Tools package and add `using Syncfusion.Windows.Forms.Tools` (C#) / `Imports Syncfusion.Windows.Forms.Tools` (VB).

Minimum programmatic setup (C#):

```csharp
// Create and add a simple RadialMenu
var radialMenu = new RadialMenu();
radialMenu.Visible = true;           // required to display
radialMenu.MenuVisibility = true;    // show items immediately
radialMenu.Style = RadialMenuStyle.Office2016Colorful;
radialMenu.Size = new Size(280, 280);
this.Controls.Add(radialMenu);

// Add items
radialMenu.Items.Add(new RadialMenuItem { Text = "New" });
radialMenu.Items.Add(new RadialMenuItem { Text = "Open" });
radialMenu.Items.Add(new RadialMenuItem { Text = "Save" });
```

VB.NET equivalent:

```vbnet
' Create and add a simple RadialMenu
Dim radialMenu As New RadialMenu()
radialMenu.Visible = True
radialMenu.MenuVisibility = True
radialMenu.Style = RadialMenuStyle.Office2016Colorful
radialMenu.Size = New Size(280, 280)
Me.Controls.Add(radialMenu)

' Add items
radialMenu.Items.Add(New RadialMenuItem() With {.Text = "New"})
radialMenu.Items.Add(New RadialMenuItem() With {.Text = "Open"})
radialMenu.Items.Add(New RadialMenuItem() With {.Text = "Save"})
```

Notes and quick tips:
- Use `ImageListAdv` for icons and set `DisplayStyle` (e.g., `ImageAboveText`).
- Set `WedgeCount` to control visible slices per level.
- For designer usage: drag `RadialMenu` from the toolbox; set properties in `InitializeComponent`.

Common minimal troubleshooting:
- Not visible → ensure `Visible = true` and `MenuVisibility = true`.
- Images not showing → attach `ImageList` before assigning `ImageIndex`.

This file focuses on the minimal, runnable setup. See other reference pages for item structure, special elements, styling, themes, and advanced features.
```
// Set these properties in the Properties window
