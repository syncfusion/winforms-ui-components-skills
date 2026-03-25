# Events and Advanced Features

## Table of Contents
- [SelectedItemCollectionChanged](#selecteditemcollectionchanged)
- [VisualItemCollectionChanged](#visualitemcollectionchanged)
- [AutoSizeModeChanged](#autosizechanged)
- [DataSourceChanged](#datasourcechanged)
- [DropDown](#dropdown)
- [Detecting VisualItemCollection Modifications](#detecting-visualitemcollection-modifications)

---

## SelectedItemCollectionChanged

Fires whenever the **selected items collection** is modified — i.e., an item is added to or removed from the selection.

### Event Arguments

| Member | Type | Description |
|---|---|---|
| `SelectedItems` | Collection | The current selected items after the change |
| `Action` | `Actions` | `Actions.Added` or `Actions.Removed` |

```csharp
private void MultiSelectionComboBox1_SelectedItemCollectionChanged(
    object sender, SelectedItemCollectionChangedArgs e)
{
    if (e.Action == Actions.Added)
    {
        string addedItem = e.SelectedItems[0].ToString();
        Console.WriteLine("Item added: " + addedItem);
    }
    else if (e.Action == Actions.Removed)
    {
        Console.WriteLine("An item was removed from selection.");
    }
}
```

```vb
Private Sub MultiSelectionComboBox1_SelectedItemCollectionChanged(
    sender As Object, e As SelectedItemCollectionChangedArgs)

    If e.Action = Actions.Added Then
        Dim text As String = e.SelectedItems(0).ToString()
        Console.WriteLine("Item added: " & text)
    End If
End Sub
```

---

## VisualItemCollectionChanged

Fires when the **VisualItem tag chips** collection changes — a tag is added or removed from the text area. This is distinct from `SelectedItemCollectionChanged` and specifically tracks the visual representation.

### Event Arguments

| Member | Type | Description |
|---|---|---|
| `VisualItems` | Collection | The current visual item tags after the change |
| `Action` | `Actions` | `Actions.Added` or `Actions.Removed` |

```csharp
private void MultiSelectionComboBox1_VisualItemsCollectionChanged(
    object sender, VisualItemCollectionChangedArgs e)
{
    if (e.Action == Actions.Added)
    {
        string tagText = e.VisualItems[0].ToString();
        Console.WriteLine("Tag added: " + tagText);
    }
}
```

```vb
Private Sub MultiSelectionComboBox1_VisualItemsCollectionChanged(
    sender As Object, e As VisualItemCollectionChangedArgs)

    If e.Action = Actions.Added Then
        Dim text As String = e.VisualItems(0).ToString()
    End If
End Sub
```

---

## AutoSizeModeChanged

Fires when the `AutoSizeMode` property is changed at runtime. Use this to react to layout changes, for example, to resize sibling controls.

### Event Arguments

| Member | Type | Description |
|---|---|---|
| `AutoSizeMode` | `AutoSizeModes` | The new AutoSizeMode value |

```csharp
void MultiSelectionComboBox1_AutoSizeStateChanged(
    object sender, AutoSizeModeEventArgs e)
{
    AutoSizeModes newMode = e.AutoSizeMode;
    Console.WriteLine("AutoSizeMode changed to: " + newMode);
}
```

```vb
Private Sub MultiSelectionComboBox1_AutoSizeStateChanged(
    sender As Object, e As AutoSizeModeEventArgs)

    Dim mode As AutoSizeModes = e.AutoSizeMode
End Sub
```

---

## DataSourceChanged

Fires when the `DataSource` property is replaced. Use it to re-apply `DisplayMember`/`ValueMember` or refresh dependent UI elements.

```csharp
private void MultiSelectionComboBox1_DataSourceChanged(object sender, EventArgs e)
{
    // Re-configure bindings if needed
    this.multiSelectionComboBox1.DisplayMember = "Name";
    this.multiSelectionComboBox1.ValueMember = "ID";
}
```

```vb
Private Sub MultiSelectionComboBox1_DataSourceChanged(
    sender As System.Object, e As System.EventArgs) _
    Handles MultiSelectionComboBox1.DataSourceChanged
    ' React to data source change
End Sub
```

---

## DropDown

Fires when the dropdown list opens or closes. The `IsDropDown` argument tells you which transition occurred.

```csharp
private void MultiSelectionComboBox1_DropDown(
    object sender, DropDownStateEventArgs e)
{
    bool isOpen = e.IsDropDown;
    Console.WriteLine("DropDown is " + (isOpen ? "open" : "closed"));
}
```

```vb
Private Sub MultiSelectionComboBox1_DropDown(
    sender As Object, e As DropDownStateEventArgs)

    Dim isDropDownOpened As Boolean = e.IsDropDown
End Sub
```

---

## Detecting VisualItemCollection Modifications

To detect when the VisualItem tag collection changes at runtime (e.g., to update a counter label), subscribe to `VisualItemCollectionChanged`:

```csharp
this.multiSelectionComboBox1.VisualItemCollectionChanged +=
    MultiSelectionComboBox1_VisualItemsCollectionChanged;

private void MultiSelectionComboBox1_VisualItemsCollectionChanged(
    object sender, VisualItemCollectionChangedArgs e)
{
    if (e.Action == Actions.Added)
    {
        labelCount.Text = "Selected: " + e.VisualItems.Count;
    }
    else if (e.Action == Actions.Removed)
    {
        labelCount.Text = "Selected: " + e.VisualItems.Count;
    }
}
```

> Use `SelectedItemCollectionChanged` when you care about the **logical selection** (the data values); use `VisualItemCollectionChanged` when you care about the **displayed tags** in the text area.
