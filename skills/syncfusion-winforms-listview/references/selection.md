# Selection

This reference documents selection APIs and patterns for `SfListView`.

## Selection modes

```csharp
sfListView1.SelectionMode = SelectionMode.MultiSimple;
```

```vbnet
sfListView1.SelectionMode = SelectionMode.MultiSimple
```

Modes: `None`, `One`, `MultiSimple`, `MultiExtended`.

## Programmatic selection

Set selected item(s) by `SelectedItem`, `SelectedIndex`, or add to `SelectedItems`:

```csharp
sfListView1.SelectedItem = sfListView1.View.DisplayItems[4];
sfListView1.SelectedIndex = 10;
foreach (var item in sfListView1.View.DisplayItems)
{
   var obj = item as CountryInfo;
   if (obj.CountryName.StartsWith("U"))
      sfListView1.SelectedItems.Add(item);
}
```

```vbnet
sfListView1.SelectedItem = sfListView1.View.DisplayItems(4)
sfListView1.SelectedIndex = 10
For Each item In sfListView1.View.DisplayItems
   Dim obj = TryCast(item, CountryInfo)
   If obj.CountryName(0).ToString() = "U" Then
      sfListView1.SelectedItems.Add(item)
   End If
Next
```

## Selected items utility

```csharp
sfListView1.SelectedItems.Clear();
int rowIndex = sfListView1.GetRowIndexAtPoint(point);
```

## Hot tracking

```csharp
sfListView1.HotTracking = true;
```

## Appearance

Customize selection colors via `sfListView1.Style.SelectionStyle`.

```csharp
sfListView1.Style.SelectionStyle.SelectionBackColor = Color.LightSeaGreen;
sfListView1.Style.SelectionStyle.SelectionForeColor = Color.DarkBlue;
```

## Events

### SelectionChanging

```csharp
sfListView1.SelectionChanging += new EventHandler<ItemSelectionChangingEventArgs>(SfListView1_SelectionChanging);
private void SfListView1_SelectionChanging(object sender, ItemSelectionChangingEventArgs e)
{
   if ((sender as SfListView).SelectedItems.Count > 0)
      e.Cancel = true;
}
```

### SelectionChanged

```csharp
sfListView1.SelectionChanged += new EventHandler<ItemSelectionChangedEventArgs>(SfListView1_SelectionChanged);
private void SfListView1_SelectionChanged(object sender, ItemSelectionChangedEventArgs e)
{
   if (e.AddedItems.Count > 0)
      (sender as SfListView).SelectedItems.Clear();
}
```

## Disabling selection for specific items

Use `SelectionChanging` to cancel selection for specific data items.

