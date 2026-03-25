# Working with AutoComplete in Windows Forms AutoComplete

This section explains how to work with various options in the AutoComplete component.

## Setting AutoComplete modes

The AutoComplete component provides the auto-complete support to the editor control based on the input text. It can be done using the `SetAutoComplete` method.

The different types of AutoComplete modes are as follows.

| AutoComplete modes | Description |
|--------------------|-------------|
| **AutoSuggest** | Suggests and displays a list of probable matches in the drop-down list by setting the AutoCompleteMode to AutoSuggest.<br>![Windows Forms AutoComplete AutoSuggest mode](../images/AutoComplete_Autosuggest.png) |
| **AutoAppend** | Appends the most appropriate match for the current content in the editor control automatically.<br>![Windows Forms AutoComplete AutoAppend mode](../images/AutoComplete_Autoappend.png) |
| **Both** | Activates both AutoAppend and AutoSuggest modes of auto completion for the editor control.<br>![Windows Forms AutoComplete Both mode](../images/AutoComplete_Both.png) |
| **Disabled** | Disables auto complete support for the editor control.<br>![Windows Forms AutoComplete Disabled mode](../images/AutoComplete_Disabled.png) |
| **MultiSuggest** | Checks whether the beginning of items in the list pop up matches with user input string. Then matched cases from various columns are shown as suggestions. The MultiSuggest mode is an extended mode of AutoSuggest.<br>![Windows Forms AutoComplete Multisuggest mode](../images/AutoComplete_Multisuggest.png) |
| **MultiSuggestExtend** | Checks whether the entered character or sequence of character is present in any part of the word in list popup item. Then matched cases from various columns are shown as suggestions.<br>![Windows Forms AutoComplete Multisuggestextended mode](../images/AutoComplete_Multisuggestextended.png) |

The following code snippet shows how to update the AutoComplete modes.

```csharp
autoComplete1.SetAutoComplete(this.textBox1, Syncfusion.Windows.Forms.Tools.AutoCompleteModes.MultiSuggestExtended);
```

```vb
autoComplete1.SetAutoComplete(Me.textBox1, Syncfusion.Windows.Forms.Tools.AutoCompleteModes.MultiSuggestExtended)
```

> **Note**: The values are filtered from the column which is set as matching column for auto complete. By default, the `MatchingColumn` value is `true` for the first column of binding source.

## Case sensitivity

Specifies whether to ignore case for string comparison. It can be enabled by setting the `CaseSensitive` property to `true`. The default value of this property is `true`.

```csharp
autoComplete1.CaseSensitive = true;
```

```vb
autoComplete1.CaseSensitive = True
```

## Match mode

The `MatchMode` property specifies the mode where the most appropriate match for the current content in the editor control is filled in the AutoComplete history list. The default value is `Automatic`.

The following code snippet implements column configuration.

```csharp
this.autoComplete1.MatchMode = AutoCompleteMatchModes.Automatic;
```

```vb
Me.autoComplete1.MatchMode = AutoCompleteMatchModes.Automatic
```

## Matching column 

The `MatchingColumn` indicates the represented column to be treated as matching column. The default value of this property is `true` for 0th index column.

Refresh column before setting MatchingColumn using the `RefreshColumns` method. 

```csharp
this.autoComplete1.RefreshColumns();
this.autoComplete1.Columns[1].MatchingColumn = true;
```

```vb
Me.autoComplete1.RefreshColumns()
Me.autoComplete1.Columns(1).MatchingColumn = true
```

## Sorting items

Specifies whether sorting needs to be performed in the items present in the AutoComplete popup. It can be done by setting the `AutoSortList` property to `true`. The default value of this property is `true`.

```csharp
autoComplete1.AutoSortList = true;
```

```vb
autoComplete1.AutoSortList = True
```

> **Note**: The items are sorted on the basis of the column whose `MatchingColumn` are set to `true`. 

## Handling duplicate values

The duplicate values can be used in AutoComplete data source by setting the `EnableDuplicateValues` property to `true`.

```csharp
autoComplete1.EnableDuplicateValues = true;
```

```vb
autoComplete1.EnableDuplicateValues = True
```

## Maintaining history of user inputs

### Adding items to history list

The current input text in the editor control can be added to the history list at run time when the Enter key is pressed. This support can be enabled by setting the `AutoAddItem` property to `true`. The default value of this property is `false`.

```csharp
autoComplete1.AutoAddItem = true;
```

```vb
autoComplete1.AutoAddItem = True
```

### Deleting items from history list

The current selected item can be removed from auto complete popup when the `Delete` key is pressed at run time. This support can be enabled by setting the `AllowListDelete` property to `true`.

```csharp
autoComplete1.AllowListDelete = true;
```

```vb
autoComplete1.AllowListDelete = True
```

### Deleting history

The history items persisted by the AutoComplete component can be deleted by invoking the `ResetHistory` method. The entire history list in the AutoComplete popup will be deleted.

```csharp
autoComplete1.ResetHistory();
```

```vb
autoComplete1.ResetHistory()
```

## Setting maximum number of suggestions

You can limit the number of suggestions need to be displayed in the AutoComplete popup using the `MaxNumberofSuggestion` property.

![Windows Forms AutoComplete Maximum number of suggestions](../images/AutoComplete_Maxsuggestions.png)

```csharp
autoComplete1.MaxNumberofSuggestion = 2;
```

```vb
autoComplete1.MaxNumberofSuggestion = 2
```

## Integration with MS ComboBox

If MS ComboBox is used as editor control, the Combobox dropdown can be suppressed and overridden by the AutoComplete component using the `OverrideCombo` property.

![Windows Forms AutoComplete Integration with MS Combobox](../images/AutoComplete_Overridecombo.png)

## Integration with RichTextBox control

The auto-complete functionality can be added to the RichTextBox control. The following steps are used to integrate the RichTextBox with the AutoComplete component:

1. Implement the `IEditControlsEmbed` interface in a CustomRichTextBox class that enables the AutoComplete functionality for the RichTextBox control.

```csharp
public class CustomRichTextBox : System.Windows.Forms.RichTextBox, IEditControlsEmbed
{
    // Returns the active RichTextBox control.
    public Control GetActiveEditControl(IEditControlsEmbedListener listener)
    {
        return (Control)this;
    }
}
```

```vb
Public Class CustomRichTextBox Inherits System.Windows.Forms.RichTextBox Implements IEditControlsEmbed
    ' Returns the active RichTextBox control.
    Public Function GetActiveEditControl(ByVal listener As IEditControlsEmbedListener) As Control
        Return CType(Me, Control)
    End Function
End Class
```

2. Create an instance for the CustomRichTextBox class and the AutoComplete component. Then, use the `SetAutoComplete` method of AutoComplete component to enable auto-complete support for the RichTextBox control.

```csharp
Syncfusion.Windows.Forms.Tools.AutoComplete autoComplete1= new Syncfusion.Windows.Forms.Tools.AutoComplete();
CustomRichTextBox richTextBox1= new CustomRichTextBox();    
autoComplete1.SetAutoComplete(richTextBox1, Syncfusion.Windows.Forms.Tools.AutoCompleteModes.AutoSuggest);
```

```vb
Dim autoComplete1 As Syncfusion.Windows.Forms.Tools.AutoComplete = New Syncfusion.Windows.Forms.Tools.AutoComplete 
Dim richTextBox1 As CustomRichTextBox = New CustomRichTextBox
autoComplete1.SetAutoComplete(richTextBox1, AutoCompleteModes.AutoSuggest)
```

![Windows Forms AutoComplete integration with Richtextbox](../images/AutoComplete_Richtextbox.png)

## Opening the AutoComplete popup programmatically

The AutoComplete popup can be shown programmatically using the `AutoCompletePopup` method. 

```csharp
autoComplete1.AutoCompletePopup.ParentControl = textBox1;
this.autoComplete1.AutoCompletePopup.ShowPopup(Point.Empty);
```

```vb
autoComplete1.AutoCompletePopup.ParentControl = textBox1
autoComplete1.AutoCompletePopup.ShowPopup(Point.Empty)
```

![Windows Forms AutoComplete programmatical popup open](../images/AutoComplete_Openpopup.png)
