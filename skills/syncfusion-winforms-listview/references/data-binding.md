# Data Binding

This reference explains data binding support for `SfListView`.

## DataSource, DisplayMember and ValueMember

Bind the list view using the `DataSource` property and control which fields are shown with `DisplayMember` and `ValueMember`.

```csharp
sfListView1.DataSource = GetDataSource();
sfListView1.DisplayMember = "CountryName";
sfListView1.ValueMember = "Continent";
```

```vbnet
sfListView1.DataSource = GetDataSource()
sfListView1.DisplayMember = "CountryName"
sfListView1.ValueMember = "Continent"
```

## Events

### View.SourcePropertyChanged

Raised when an item in the bound data raises `INotifyPropertyChanged` for a property. Handler signature:

```csharp
sfListView1.View.SourcePropertyChanged += new PropertyChangedEventHandler(View_SourcePropertyChanged);
private void View_SourcePropertyChanged(object sender, PropertyChangedEventArgs e)
{
	// e.PropertyName
}
```

```vbnet
AddHandler sfListView1.View.SourcePropertyChanged, AddressOf View_SourcePropertyChanged
Private Sub View_SourcePropertyChanged(ByVal sender As Object, ByVal e As PropertyChangedEventArgs)
	' e.PropertyName
End Sub
```

### View.SourceCollectionChanged

Raised when the bound collection changes (add/remove/move/replace/reset). Handler signature:

```csharp
sfListView1.View.SourceCollectionChanged += new NotifyCollectionChangedEventHandler(View_SourceCollectionChanged);
private void View_SourceCollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
{
	// e.Action, e.NewItems, e.OldItems
}
```

```vbnet
AddHandler sfListView1.View.SourceCollectionChanged, AddressOf View_SourceCollectionChanged
Private Sub View_SourceCollectionChanged(ByVal sender As Object, ByVal e As NotifyCollectionChangedEventArgs)
	' e.Action, e.NewItems, e.OldItems
End Sub
```

## Notes

- Prefer `ObservableCollection<T>` or other INotifyCollectionChanged implementations for live updates.
- Call `sfListView1.View.RefreshFilter()` after changing filter predicates.

