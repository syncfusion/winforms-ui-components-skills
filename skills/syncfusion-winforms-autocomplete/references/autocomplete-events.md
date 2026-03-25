# Autocomplete Events in Windows Forms AutoComplete

The events of the AutoComplete component are as follows.

| AutoComplete events | Description |
|---------------------|-------------|
| `BeforeAddItem` | Handles when a new item is about to be added. |
| `AutoCompleteCustomize` | Handles to customize the auto completion. |
| `AutoCompleteItemBrowsed` | Handles when you select an item from the list of possible matches when the AutoComplete is set to AutoSuggest. |
| `AutoCompleteItemSelected` | Occurs when a new item is selected when the AutoComplete mode is set to AutoSuggest. |
| `DropDownClosed` | Occurs when the AutoComplete dropdown is closed. |
| `DropDownDisplayed` | Occurs when the AutoComplete dropdown is displayed. |
| `MatchItem` | Provides a custom matching routine for the current value in the editor control. |
| `PreMatchItem` | Handles before the AutoComplete component performs a matching operation for the current text content of the active editor control. |
| `TargetChanging` | Occurs when the editor control of the AutoComplete component changes. |

## AutoCompleteItemSelected event

This event occurs while selecting a new item when the AutoComplete mode is set to AutoSuggest.

```csharp
private void autoComplete1_AutoCompleteItemSelected(object sender, AutoCompleteItemEventArgs args)
{
    string selectedItem = args.ItemArray[0].ToString();
    MessageBox.Show("Selected: " + selectedItem);
}
```

## BeforeAddItem Event

This event occurs when a new item is about to be added. New items can be added by calling the AutoComplete.AddHistoryItem() method. The event handler receives an argument of type AutoCompleteAddItemCancelEventArgs that contains data related to this event. The following are the properties associated with the AutoCompleteAddItemCancelEventArgs argument.

| Members | Description |
|---------|-------------|
| `Cancel` | Gets or sets a value that indicates whether the event should be canceled. |
| `ImageColumnIndex` | Gets or sets the ColumnIndex into the AutoComplete.ImageList property. |
| `RowItem` | It is the System.Data.DataRow object that contains the value that is to be added to the history list. |

```csharp
private void autoComplete1_BeforeAddItem(object sender, AutoCompleteAddItemCancelEventArgs e)
{
    // Cancels the item that is going to be added.
    e.Cancel = true;
}
```

```vb
Private Sub autoComplete1_BeforeAddItem(ByVal sender As Object, ByVal e As AutoCompleteAddItemCancelEventArgs)
    'Cancels the item that is going to be added.   
    e.Cancel = True
End Sub
```

## AutoCompleteItemBrowsed event

This event occurs when selecting an item from the list of possible matches when the AutoComplete is set to AutoSuggest. The event handler receives an argument of type AutoCompleteItemEventArgs. The event properties associated with the AutoCompleteItemEventArgs are as follows.

| Members | Description |
|---------|-------------|
| `SelectedValue` | Gets or sets the value selected. |
| `Handled` | Specifies whether SelectedValue should be applied to editor control. This can be used only with AutoCompleteItemSelected. |
| `ItemArray` | Returns AutoComplete item as an object array. |
| `MatchColumnIndex` | Returns the index of the item that was used for matching. |

When you select an item from the list of possible matches when AutoComplete is set to AutoSuggest, you can display the selected URL in a separate TextBox. The following code sample demonstrates this.

```csharp
private void autoComplete1_AutoCompleteItemBrowsed(object sender, Syncfusion.Windows.Forms.Tools.AutoCompleteItemEventArgs args)
{
    string itemText = args.ItemArray[0].ToString();
    string eventlogmessage = String.Format("Event: {0} Item: {1}\r\n", "AutoCompleteItemSelected", itemText);
    textBox1.Text = textBox1.Text + eventlogmessage;
}
```

```vb
Private Sub autoComplete1_AutoCompleteItemBrowsed(ByVal sender As Object, ByVal args As Syncfusion.Windows.Forms.Tools.AutoCompleteItemEventArgs)
    Dim itemText As String = args.ItemArray(0).ToString()
    Dim eventlogmessage As String = [String].Format("Event: {0} Item: {1}" & Chr(13) & "" & Chr(10) & "", "AutoCompleteItemSelected", itemText)
    textBox1.Text = textBox1.Text + eventlogmessage
End Sub
```

## MatchItem event

You can override the default matching of the current content of the editor control using this event. The event handler receives an argument of type AutoCompleteMatchItemEventArgs. The following are the properties associated with AutoCompleteMatchItemEventArgs argument.

| Members | Description |
|---------|-------------|
| `Cancel` | Gets or sets a value that indicates whether the event should be canceled. |
| `CurrentText` | Returns the current text value to be matched. |
| `PossibleMatch` | Returns the possible match value that needs to be compared against AutoCompleteMatchItemEventArgs.CurrentText by the event handler. |

```csharp
private void autoComplete1_MatchItem(object sender, AutoCompleteMatchItemEventArgs args)
{
    // Cancels the match operation.
    args.Cancel = true;
}
```

```vb
Private Sub autoComplete1_MatchItem(ByVal sender As Object, ByVal args As AutoCompleteMatchItemEventArgs)
    ' Cancels the match operation.
    args.Cancel = True
End Sub
```

## DropDownDisplayed event

This event is triggered when the AutoComplete dropdown is displayed.

```csharp
private void autoComplete1_DropDownDisplayed(object sender, EventArgs e)
{
    Console.WriteLine("Dropdown displayed");
}
```

## DropDownClosed event

This event is triggered when the AutoComplete dropdown is closed.

```csharp
private void autoComplete1_DropDownClosed(object sender, EventArgs e)
{
    Console.WriteLine("Dropdown closed");
}
```

## Best Practices for Event Handling

1. **Use AutoCompleteItemSelected**: Use this event to respond to user selections
2. **Validate with BeforeAddItem**: Prevent invalid items from being added
3. **Custom matching**: Use MatchItem for complex matching logic
4. **Track dropdown state**: Use DropDownDisplayed and DropDownClosed for UI synchronization
5. **Handle exceptions**: Always wrap event handlers with try-catch blocks
6. **Avoid heavy operations**: Keep event handlers lightweight to maintain responsiveness
