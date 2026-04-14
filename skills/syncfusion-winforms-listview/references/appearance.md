# Appearance

This reference explains appearance customization for `SfListView`.

## Item appearance

Use `sfListView1.Style.ItemStyle` to configure per-item visuals.

```csharp
sfListView1.Style.ItemStyle.BackColor = Color.AliceBlue;
sfListView1.Style.ItemStyle.ForeColor = Color.DarkBlue;
sfListView1.Style.ItemStyle.TextAlignment = ContentAlignment.BottomLeft;
sfListView1.Style.ItemStyle.Font = new Font("Segoe UI", 10);
```

```vbnet
sfListView1.Style.ItemStyle.BackColor = Color.AliceBlue
sfListView1.Style.ItemStyle.ForeColor = Color.DarkBlue
sfListView1.Style.ItemStyle.TextAlignment = ContentAlignment.BottomLeft
sfListView1.Style.ItemStyle.Font = New Font("Segoe UI", 10)
```

## Group appearance

Configure group header visuals via `sfListView1.Style.GroupItemStyle`.

```csharp
sfListView1.Style.GroupItemStyle.BackColor = Color.PaleTurquoise;
sfListView1.Style.GroupItemStyle.ForeColor = Color.DarkRed;
sfListView1.Style.GroupItemStyle.TextAlignment = ContentAlignment.BottomLeft;
sfListView1.Style.GroupItemStyle.Font = new Font("Segoe UI", 12);
```

```vbnet
sfListView1.Style.GroupItemStyle.BackColor = Color.PaleTurquoise
sfListView1.Style.GroupItemStyle.ForeColor = Color.DarkRed
sfListView1.Style.GroupItemStyle.TextAlignment = ContentAlignment.BottomLeft
sfListView1.Style.GroupItemStyle.Font = New Font("Segoe UI", 12)
```

## Conditional styling (DrawItem)

Use the `DrawItem` event to apply per-item conditional styling.

```csharp
sfListView1.DrawItem += new EventHandler<Syncfusion.WinForms.ListView.Events.DrawItemEventArgs>(SfListView1_DrawItem);
private void SfListView1_DrawItem(object sender, DrawItemEventArgs e)
{
    if(e.ItemType == ItemType.Record && (e.ItemData as CountryInfo).Continent == "Asia")
    {
        e.Style.BackColor = Color.Coral;
        e.Style.ForeColor = Color.White;
    }
}
```

```vbnet
AddHandler sfListView1.DrawItem, AddressOf SfListView1_DrawItem
Private Sub SfListView1_DrawItem(ByVal sender As Object, ByVal e As DrawItemEventArgs)
    If e.ItemType Is ItemType.Record AndAlso (TryCast(e.ItemData, CountryInfo)).Continent = "Asia" Then
        e.Style.BackColor = Color.Coral
        e.Style.ForeColor = Color.White
    End If
End Sub
```

## Images for items

You can set `e.Image` inside `DrawItem` to add images to items. Avoid hard-coded relative paths in examples; prefer resource loading or embedded assets.

## Themes and SfSkinManager

`SfListView` supports built-in themes such as `Office2016Colorful`, `Office2016White`, `Office2016DarkGray`, and `Office2016Black`.

Load the theme assembly once at application startup before creating forms. Example:

```csharp
using Syncfusion.WinForms.Controls;

static class Program
{
    [STAThread]
    static void Main()
    {
        SfSkinManager.LoadAssembly(typeof(Office2016Theme).Assembly);
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());
    }
}
```

VB.NET equivalent:

```vbnet
Imports Syncfusion.WinForms.Controls

Module Program
    <STAThread>
    Sub Main()
        SfSkinManager.LoadAssembly(GetType(Office2016Theme).Assembly)
        Application.EnableVisualStyles()
        Application.SetCompatibleTextRenderingDefault(False)
        Application.Run(New Form1())
    End Sub
End Module
```

Apply a theme by setting `ThemeName`:

```csharp
sfListView1.ThemeName = "Office2016Colorful";
```

```vbnet
sfListView1.ThemeName = "Office2016Colorful"
```

## Notes

- Removed inline image references from this reference; examples should link to project resources or screenshots hosted in the docs site.
- Prefer `SfSkinManager.LoadAssembly` once at startup — do not call it repeatedly.

